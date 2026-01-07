# 06\. 只追加键值存储

## 6.1 我们将要做的事情

我们将创建一个基于文件支持的写时复制 B+树的键值存储。

```go
type KV struct {
    Path   string // file name
    // internals
    fd   int
    tree BTree
    // more ...
}
func (db *KV) Open() error

func (db *KV) Get(key []byte) ([]byte, bool) {
    return db.tree.Get(key)
}
func (db *KV) Set(key []byte, val []byte) error {
    db.tree.Insert(key, val)
    return updateFile(db)
}
func (db *KV) Del(key []byte) (bool, error) {
    deleted := db.tree.Delete(key)
    return deleted, updateFile(db)
}
```

本章的范围是持久性和原子性：

+   文件是只追加的；空间重用留到下一章讨论。

+   我们将忽略并发，并假设在 1 个进程内进行顺序访问。

我们将实现 3 个处理磁盘页面的 B+树回调：

```go
type BTree struct {
    root uint64
    get func(uint64) []byte // read a page
    new func([]byte) uint64 // append a page
    del func(uint64)        // ignored in this chapter
}
```

## 6.2 两阶段更新

### 原子性 + 持久性

如第三章所述，对于写时复制的树，根指针是原子更新的。然后使用`fsync`来*请求*和*确认*持久性。

根指针本身的原子性不足；为了使整个树原子化，必须在根指针之前持久化新节点。而且**写入顺序不是数据持久化的顺序**，这是由于缓存等因素。因此，又使用了另一个`fsync`来确保顺序。

```go
func updateFile(db *KV) error {
    // 1\. Write new nodes.
    if err := writePages(db); err != nil {
        return err
    }
    // 2\. `fsync` to enforce the order between 1 and 3.
    if err := syscall.Fsync(db.fd); err != nil {
        return err
    }
    // 3\. Update the root pointer atomically.
    if err := updateRoot(db); err != nil {
        return err
    }
    // 4\. `fsync` to make everything persistent.
    return syscall.Fsync(db.fd)
}
```

### 替代方案：带日志的持久性

替代的双重写入方案也有 2 个`fsync`阶段：

1.  使用校验和编写更新后的页面。

1.  使用`fsync`使更新持久化（用于崩溃恢复）。

1.  在原地更新数据（应用双重写入）。

1.  用于 3 和 1 之间顺序的`fsync`（重用或删除双重写入）。

与写时复制不同，阶段顺序是：数据在第一个`fsync`后持久化；数据库可以返回成功并在后台完成其余操作。

双重写入与日志类似，更新也只需要 1 个`fsync`。它还可以是一个实际的日志来缓冲多个更新，这提高了性能。这是数据库中日志的另一个例子，除了 LSM 树。

我们不会使用日志，因为写时复制不需要它。但日志仍然提供了上述讨论的好处；这是日志在数据库中无处不在的原因之一。

### 内存数据的并发性

内存数据（关于并发）的原子性可以通过互斥锁（锁定）或某些原子 CPU 指令来实现。存在一个类似的问题：由于乱序执行等因素，内存读取/写入可能不会按顺序出现。

对于内存中的写时复制树，必须在更新根指针之前使新节点对并发读取者可见。这被称为*内存屏障*，它与`fsync`类似，尽管`fsync`不仅仅是强制执行顺序。

同步原语，如互斥锁，或任何操作系统系统调用，将以可移植的方式强制执行内存排序，因此你不必与特定于 CPU 的原子操作或屏障（这些对于并发来说根本不足够）纠缠。

## 6.3 文件上的数据库

### 文件布局

我们的数据库是一个分为“页面”的单个文件。每个页面是一个 B+树节点，除了第一个页面；第一个页面包含指向最新根节点的指针和其他辅助数据，我们称这个为*元页面*。

```go
|     the_meta_page    | pages... | root_node | pages... | (end_of_file)
| root_ptr | page_used |                ^                ^
      |          |                      |                |
      +----------|----------------------+                |
                 |                                       |
                 +---------------------------------------+
```

新节点就像日志一样简单追加，但我们不能使用文件大小来计算页数，因为断电后文件大小（元数据）可能与文件数据不一致。这取决于文件系统，我们可以通过在元页面中存储页数来避免这种情况。

### 在目录上执行 `fsync`

如第一章所述，在 `rename` 之后必须在父目录上使用 `fsync`。在创建新文件时也是如此，因为有两个东西需要持久化：文件数据和引用文件的目录。

我们将在使用 `O_CREATE` 可能创建新文件后预先执行 `fsync`。要执行目录上的 `fsync`，请以 `O_RDONLY` 模式打开目录。

```go
func createFileSync(file string) (int, error) {
    // obtain the directory fd
    flags := os.O_RDONLY | syscall.O_DIRECTORY
    dirfd, err := syscall.Open(path.Dir(file), flags, 0o644)
    if err != nil {
        return -1, fmt.Errorf("open directory: %w", err)
    }
    defer syscall.Close(dirfd)
    // open or create the file
    flags = os.O_RDWR | os.O_CREATE
    fd, err := syscall.Openat(dirfd, path.Base(file), flags, 0o644)
    if err != nil {
        return -1, fmt.Errorf("open file: %w", err)
    }
    // fsync the directory
    if err = syscall.Fsync(dirfd); err != nil {
        _ = syscall.Close(fd)  // may leave an empty file
        return -1, fmt.Errorf("fsync directory: %w", err)
    }
    return fd, nil
}
```

目录文件描述符可以被 `openat` 使用来打开目标文件，这保证了文件来自我们之前打开的同一目录，以防目录路径在之间被替换（竞争条件）。尽管这不是我们的关注点，因为我们不期望有多个进程的操作。

### `mmap`、页面缓存和 I/O

`mmap` 是一种将文件读写操作当作内存缓冲区来处理的方法。使用 `mmap` 时，磁盘 I/O 是隐式和自动的。

```go
func Mmap(fd int, offset int64, length int, ...) (data []byte, err error)
```

要理解 `mmap`，让我们回顾一些操作系统的基础知识。操作系统中的页面是虚拟地址和物理地址映射的最小单位。然而，进程的虚拟地址空间并不总是完全由物理内存支持；进程的一部分内存可以被交换到磁盘上，当进程尝试访问它时：

1.  CPU 触发一个 *页面错误*，然后将控制权交给操作系统。

1.  然后，操作系统 ...

    1.  将交换的数据读入物理内存。

    1.  将虚拟地址重新映射到它。

    1.  将控制权交还给进程。

1.  进程以映射到真实 RAM 的虚拟地址恢复。

`mmap` 的工作方式类似，进程从 `mmap` 获取一个地址范围，当它访问其中的一个页面时，会发生页面错误，操作系统将数据读入缓存并将页面重新映射到缓存。这就是只读场景中的自动 I/O。

CPU 也会记录（称为脏位）进程修改页面时的信息，以便操作系统可以在稍后将其写回磁盘。`fsync` 用于请求并等待 I/O。这是通过 `mmap` 写入数据，它与 Linux 中的 `write` 操作没有太大区别，因为 `write` 也是写入相同的页面缓存。

你不必使用 `mmap`，但理解其基础很重要。

## 6.4 管理磁盘页面

我们将使用 `mmap` 来实现这些页面管理回调，因为它很方便。

```go
func (db *KV) Open() error {
    db.tree.get = db.pageRead   // read a page
    db.tree.new = db.pageAppend // apppend a page
    db.tree.del = func(uint64) {}
    // ...
}
```

### 调用 `mmap`

文件支持的 `mmap` 可以是只读、读写或写时复制。要创建只读 `mmap`，请使用 `PROT_READ` 和 `MAP_SHARED` 标志。

```go
syscall.Mmap(fd, offset, size, syscall.PROT_READ, syscall.MAP_SHARED)
```

映射的范围可以大于当前文件大小，这是一个我们可以利用的事实，因为文件会增长。

### `mmap` 一个正在增长的文件

`mremap` remaps a mapping to a larger range, it’s like `realloc`. That’s one way to deal with the growing file. However, the address may change, which can hinder concurrent readers in later chapters. Our solution is to add new mappings to cover the expanded file.

```go
type KV struct {
    // ...
    mmap struct {
        total  int      // mmap size, can be larger than the file size
        chunks [][]byte // multiple mmaps, can be non-continuous
    }
}

// `BTree.get`, read a page.
func (db *KV) pageRead(ptr uint64) []byte {
    start := uint64(0)
    for _, chunk := range db.mmap.chunks {
        end := start + uint64(len(chunk))/BTREE_PAGE_SIZE
        if ptr < end {
            offset := BTREE_PAGE_SIZE * (ptr - start)
            return chunk[offset : offset+BTREE_PAGE_SIZE]
        }
        start = end
    }
    panic("bad ptr")
}
```

Adding a new mapping each time the file is expanded results in lots of mappings, which is bad for performance because the OS has to keep track of them. This is avoided with exponential growth, since `mmap` can go beyond the file size.

```go
func extendMmap(db *KV, size int) error {
    if size <= db.mmap.total {
        return nil // enough range
    }
    alloc := max(db.mmap.total, 64<<20) // double the current address space
    for db.mmap.total + alloc < size {
        alloc *= 2 // still not enough?
    }
    chunk, err := syscall.Mmap(
        db.fd, int64(db.mmap.total), alloc,
        syscall.PROT_READ, syscall.MAP_SHARED, // read-only
    )
    if err != nil {
        return fmt.Errorf("mmap: %w", err)
    }
    db.mmap.total += alloc
    db.mmap.chunks = append(db.mmap.chunks, chunk)
    return nil
}
```

你可能会 wonder 为什么不 just create a very large mapping (say, 1TB) and forget about the growing file, since an unrealized virtual address costs nothing. This is OK for a toy DB in 64-bit systems.

### Capture page updates

The `BTree.new` callback collects new pages from B+tree updates, and allocates the page number from the end of DB.

```go
type KV struct {
    // ...
    page struct {
        flushed uint64   // database size in number of pages
        temp    [][]byte // newly allocated pages
    }
}

func (db *KV) pageAppend(node []byte) uint64 {
    ptr := db.page.flushed + uint64(len(db.page.temp)) // just append
    db.page.temp = append(db.page.temp, node)
    return ptr
}
```

Which are written (appended) to the file after B+tree updates.

```go
func writePages(db *KV) error {
    // extend the mmap if needed
    size := (int(db.page.flushed) + len(db.page.temp)) * BTREE_PAGE_SIZE
    if err := extendMmap(db, size); err != nil {
        return err
    }
    // write data pages to the file
    offset := int64(db.page.flushed * BTREE_PAGE_SIZE)
    if _, err := unix.Pwritev(db.fd, db.page.temp, offset); err != nil {
        return err
    }
    // discard in-memory data
    db.page.flushed += uint64(len(db.page.temp))
    db.page.temp = db.page.temp[:0]
    return nil
}
```

`pwritev` is variant of `write` with an offset and multiple input buffers. We have to control the offset because we also need to write the meta page later. Multiple input buffers are combined by the kernel.

## 6.5 元数据页

### Read the meta page

We’ll also add some magic bytes to the meta page to identify the file type.

```go
const DB_SIG = "BuildYourOwnDB06" // not compatible between chapters

// | sig | root_ptr | page_used |
// | 16B |    8B    |     8B    |
func saveMeta(db *KV) []byte {
    var data [32]byte
    copy(data[:16], []byte(DB_SIG))
    binary.LittleEndian.PutUint64(data[16:], db.tree.root)
    binary.LittleEndian.PutUint64(data[24:], db.page.flushed)
    return data[:]
}

func loadMeta(db *KV, data []byte)
```

The meta page is reserved if the file is empty.

```go
func readRoot(db *KV, fileSize int64) error {
    if fileSize == 0 { // empty file
        db.page.flushed = 1 // the meta page is initialized on the 1st write
        return nil
    }
    // read the page
    data := db.mmap.chunks[0]
    loadMeta(db, data)
    // verify the page
    // ...
    return nil
}
```

### Update the meta page

Writing a small amount of page-aligned data to a real disk, modifying only a single sector, is likely power-loss-atomic at the hardware level. Some [real databases](https://www.postgresql.org/message-id/flat/17064-bb0d7904ef72add3%40postgresql.org) depend on this. That’s how we update the meta page too.

```go
// 3\. Update the meta page. it must be atomic.
func updateRoot(db *KV) error {
    if _, err := syscall.Pwrite(db.fd, saveMeta(db), 0); err != nil {
        return fmt.Errorf("write meta page: %w", err)
    }
    return nil
}
```

However, atomicity means different things at different levels, as you’ve seen with `rename`. `write` is not atomic w.r.t. concurrent readers at the [system call level](https://stackoverflow.com/questions/35595685/). This is likely how the page cache works.

We’ll consider read/write atomicity when we add concurrent transactions, but we have already seen a solution: In an LSM-tree, the 1st level is the only thing that is updated, and it’s duplicated as a MemTable, which moves the concurrency problem to memory. We can keep an in-memory copy of the meta page and synchronize it with a mutex, thus avoiding concurrent disk reads/writes.

Even if the hardware is not atomic w.r.t. power loss. Atomicity is achievable with log + checksum. We could switch between 2 checksumed meta pages for each update, to ensure that one of them is good after a power loss. This is called *double buffering*, which is a rotating log with 2 entries.

## 6.6 错误处理

### IO 错误后的场景

The bare minimum of error handling is to propagate errors with `if err != nil`. Next, consider the possibility of using the DB after an IO error (`fsync` or `write`).

+   更新失败后读取？

    +   The reasonable choice is to behave as if nothing happened.

+   Update it again after a failure?

    +   If the error persists, it’s expected to fail again.

    +   If the error is temporary, can we recover from the previous error?

+   问题解决后重启数据库？

    +   这只是崩溃恢复；在第三章中讨论。

### 回滚到上一个版本

有关于处理 `fsync` 失败的[调查](https://www.usenix.org/system/files/atc20-rebello.pdf)。从中我们可以了解到，这个话题是依赖于文件系统的。如果我们读取 `fsync` 失败后的数据，一些文件系统会返回失败的数据，因为页面缓存与磁盘不匹配。所以读取失败的写入数据是有问题的。

但由于我们是副本写入，这不是问题；我们可以回滚到旧的树根以避免有问题的数据。树根存储在元页中，但我们一旦打开数据库后，就永远不会从磁盘读取元页，所以我们只需回滚到*内存中的*根指针。

```go
func (db *KV) Set(key []byte, val []byte) error {
    meta := saveMeta(db) // save the in-memory state (tree root)
    db.tree.Insert(key, val)
    return updateOrRevert(db, meta)
}

func updateOrRevert(db *KV, meta []byte) error {
    // 2-phase update
    err := updateFile(db)
    // revert on error
    if err != nil {
        // the in-memory states can be reverted immediately to allow reads
        loadMeta(db, meta)
        // discard temporaries
        db.page.temp = db.page.temp[:0]
    }
    return err
}
```

因此，在写入失败后，仍然可以使用数据库的只读模式。读取也可能失败，但我们使用 `mmap`，在读取错误时，进程会被 `SIGBUS` 终止。这是 `mmap` 的一个缺点。

### 从临时写入错误中恢复

一些写入错误是临时的，例如“空间不足”。如果更新失败然后下一个成功，最终状态仍然是好的。问题是中间状态：在两次更新之间，磁盘上的元页内容是未知的！

如果 `fsync` 在元页上失败，磁盘上的元页可以是新版本或旧版本，而内存中的树根是旧版本。所以第二次成功的更新将覆盖新版本的数据库页，如果崩溃，可能会留下损坏的中间状态。

解决方案是在恢复时重写最后已知的元页。

```go
type KV struct {
    // ...
    failed bool // Did the last update fail?
}

func updateOrRevert(db *KV, meta []byte) error {
    // ensure the on-disk meta page matches the in-memory one after an error
    if db.failed {
        // write and fsync the previous meta page
        // ...
        db.failed = false
    }
    err := updateFile(db)
    if err != nil {
        // the on-disk meta page is in an unknown state;
        // mark it to be rewritten on later recovery.
        db.failed = true
        // ...
    }
    return err
}
```

我们依赖于文件系统正确报告错误，但有[证据](https://danluu.com/filesystem-errors/)表明它们并不总是这样做。所以整个系统能否处理错误仍然是个疑问。

## 6.7 只追加 KV 存储的总结

+   B+树副本写入的文件布局。

+   使用 `fsync` 的持久性和原子性。

+   错误处理。

磁盘上的 B+树是一个重要的步骤。我们只需添加一个空闲列表使其变得实用。
