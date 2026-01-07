# 07. 自由列表：回收与重用

KV 存储的最后一步是重用已删除的页面，这也是内存数据结构的一个问题。

## 7.1 内存管理技术

### 我们将要做什么

内存（空间）管理可以是手动或自动的。垃圾收集器是自动的，它不需要程序员的任何帮助就能检测到未使用的对象。下一个问题是如何处理（重用）未使用的对象。

我们不需要 GC，因为在树形数据结构中，检测未使用节点是微不足道的，就像我们已经用`BTree.del`回调函数所做的那样。我们将重新实现这些回调函数。

### 未使用对象列表

管理磁盘空间以相同大小的*页面*进行的原因之一是，它们在被删除后变得*可交换*；数据库在需要页面时可以重用任何一个。这比处理任意大小的通用内存管理例程，如`malloc`，要简单。

我们需要存储一个未使用页面的列表，称为*自由列表*或*对象池*。对于内存数据，这可以简单地是一个指针数组，或者嵌入在对象中的链表。

### 内嵌链表

最简单的方案是使用内嵌（侵入式）链表。链表指针位于对象本身内部；它借用对象的空间，因此不需要为数据结构额外空间。

```go
 head
   ↓
[ next | space... ] (unused object 1)
   ↓
[ next | space... ] (unused object 2)
   ↓
  ...
```

然而，这与写时复制冲突，因为它在更新期间会覆盖。

### 外部列表

另一种方案是将未使用页面的指针存储在外部数据结构中。外部数据结构本身占用空间，这是我们将在后面解决的问题。

假设我们的自由列表只是一个未使用页面编号的日志；添加项只是追加。问题是如何删除项，使其不会无限增长。

## 7.2 磁盘上的链表

### 自由列表需求

让我们想象自由列表是一个项目序列，就像一个日志。在写时复制树中，每次更新都需要新节点并删除旧节点，因此自由列表在每个更新中都会被添加和删除。如果从末尾删除项，则添加的项会*覆盖*旧数据，需要额外的崩溃恢复机制，这在第三章中讨论过。

如果从开始删除项，如何回收被删除项的空间？我们又回到了原始问题。

为了解决这个问题，自由列表也应该基于页面，这样它就可以自我管理。基于页面的列表只是一个链表，除了一个页面可以持有多个项，就像 B+树节点一样。这也被称为*展开链表*。

总结：

+   我们的自由列表是一个独立的数据结构：页面的链表。

    +   当它生长一个新节点时，它将尝试从自身获取一个页面。

    +   被删除的列表节点被添加到自身以供重用。

+   每个页面可以包含多个项（页面编号）。

    +   页面在原地更新，但页面内仍然是*追加只读*。

+   项目被添加到尾节点，并从头部消费。

    +   这样更容易使尾节点只允许追加。

### 自由列表磁盘布局

每个节点都以指向下一个节点的指针开始。项目被附加到它的旁边。

```go
// node format:
// | next | pointers | unused |
// |  8B  |   n*8B   |   ...  |
type LNode []byte

const FREE_LIST_HEADER = 8
const FREE_LIST_CAP = (BTREE_PAGE_SIZE - FREE_LIST_HEADER) / 8

// getters & setters
func (node LNode) getNext() uint64
func (node LNode) setNext(next uint64)
func (node LNode) getPtr(idx int) uint64
func (node LNode) setPtr(idx int, ptr uint64)
```

我们还在元页面中存储了头节点和尾节点的指针。尾节点的指针对于 O(1)插入是必需的。

```go
 first_item
                         ↓
head_page -> [ next |    xxxxx ]
                ↓
             [ next | xxxxxxxx ]
                ↓
tail_page -> [ NULL | xxxx     ]
                         ↑
                     last_item
```

### 更新自由列表节点

没有自由列表，元页面是唯一一个进行就地更新的页面，这就是为什么写时复制使得崩溃恢复变得容易。现在在列表节点中还有 2 个就地页面更新：下一个指针和附加的项目。

虽然列表节点是在就地更新的，但页面内没有数据被覆盖。所以如果更新被中断，元页面仍然指向相同的数据；不需要额外的崩溃恢复。而且与元页面不同，不需要原子性。

通过这次分析，嵌入式列表也可以工作，如果 B+树节点中保留下一个指针的话。这里你可以偏离书本。尽管这加倍了写放大。

## 7.3 自由列表实现

### 自由列表接口

```go
type KV struct {
    Path   string
    // internals
    fd   int
    tree BTree
    free FreeList // added
    // ...
}
```

`FreeList` 是 `KV` 中的额外数据结构。

```go
type FreeList struct {
    // callbacks for managing on-disk pages
    get func(uint64) []byte // read a page
    new func([]byte) uint64 // append a new page
    set func(uint64) []byte // update an existing page
    // persisted data in the meta page
    headPage uint64 // pointer to the list head node
    headSeq  uint64 // monotonic sequence number to index into the list head
    tailPage uint64
    tailSeq  uint64
    // in-memory states
    maxSeq uint64 // saved `tailSeq` to prevent consuming newly added items
}

// get 1 item from the list head. return 0 on failure.
func (fl *FreeList) PopHead() uint64
// add 1 item to the tail
func (fl *FreeList) PushTail(ptr uint64)
```

就像 `BTree` 一样，页面管理通过 3 个回调进行隔离：

+   `get` 读取一个页面；与之前相同，

+   `new` 添加一个页面；之前用于 `BTree`。

+   `set` 返回一个可写缓冲区以捕获就地更新。

+   `del` 不存在，因为自由列表自己管理空闲页面。

```go
func (db *KV) Open() error {
    // ...
    // B+tree callbacks
    db.tree.get = db.pageRead      // read a page
    db.tree.new = db.pageAlloc     // (new) reuse from the free list or append
    db.tree.del = db.free.PushTail // (new) freed pages go to the free list
    // free list callbacks
    db.free.get = db.pageRead      // read a page
    db.free.new = db.pageAppend    // append a page
    db.free.set = db.pageWrite     // (new) in-place updates
    // ...
}
```

### 自由列表数据结构

由于一个节点包含的项目数量可变，最多到 `FREE_LIST_CAP`，我们需要知道第一个项目在头节点中的位置（`headSeq`），以及项目在尾节点中的结束位置（`tailSeq`）。

```go
type FreeList struct {
    // ...
    // persisted data in the meta page
    headPage uint64 // pointer to the list head node
    headSeq  uint64 // monotonic sequence number to index into the list head
    tailPage uint64
    tailSeq  uint64
    // in-memory states
    maxSeq uint64 // saved `tailSeq` to prevent consuming newly added items
}
```

`headSeq`、`tailSeq` 是头节点和尾节点的索引，除了它们是单调递增的。所以循环索引是：

```go
func seq2idx(seq uint64) int {
    return int(seq % FREE_LIST_CAP)
}
```

我们使它们单调递增，这样它们就成为了列表位置的唯一标识符；为了防止列表头超过列表尾，只需比较两个序列号。

在更新期间，列表既被添加也被移除，当我们从头部移除时，我们不能移除刚刚添加到尾部的项目。所以我们需要 …

1.  在更新的开始，将原始的 `tailSeq` 保存到 `maxSeq`。

1.  在更新期间，`headSeq` 不能超过 `maxSeq`。

1.  在下一次更新的开始，`maxSeq` 被推进到 `tailSeq`。

1.  …

```go
// make the newly added items available for consumption
func (fl *FreeList) SetMaxSeq() {
    fl.maxSeq = fl.tailSeq
}
```

### 从自由列表中消费

从头节点移除一个项目只是简单地前进 `headSeq`。当头节点变为空时，移动到下一个节点。

```go
// remove 1 item from the head node, and remove the head node if empty.
func flPop(fl *FreeList) (ptr uint64, head uint64) {
    if fl.headSeq == fl.maxSeq {
        return 0, 0 // cannot advance
    }
    node := LNode(fl.get(fl.headPage))
    ptr = node.getPtr(seq2idx(fl.headSeq)) // item
    fl.headSeq++
    // move to the next one if the head node is empty
    if seq2idx(fl.headSeq) == 0 {
        head, fl.headPage = fl.headPage, node.getNext()
        assert(fl.headPage != 0)
    }
    return
}
```

自由列表自我管理；被移除的头节点被反馈给自己。

```go
// get 1 item from the list head. return 0 on failure.
func (fl *FreeList) PopHead() uint64 {
    ptr, head := flPop(fl)
    if head != 0 { // the empty head node is recycled
        fl.PushTail(head)
    }
    return ptr
}
```

如果最后一个节点被移除怎么办？一个包含 0 个节点的链表意味着讨厌的特殊情况。在实践中，**设计链表至少有 1 个节点**比处理特殊情况更容易。这就是为什么我们 `assert(fl.headPage != 0)`。

### 将项目推入自由列表

将一个项目添加到尾节点只是简单地前进 `tailSeq`。当尾节点满时，我们立即添加一个新的空尾节点，以确保至少有 1 个节点，以防先前的尾节点被移除作为头节点。

```go
func (fl *FreeList) PushTail(ptr uint64) {
    // add it to the tail node
    LNode(fl.set(fl.tailPage)).setPtr(seq2idx(fl.tailSeq), ptr)
    fl.tailSeq++
    // add a new tail node if it's full (the list is never empty)
    if seq2idx(fl.tailSeq) == 0 {
        // try to reuse from the list head
        next, head := flPop(fl) // may remove the head node
        if next == 0 {
            // or allocate a new node by appending
            next = fl.new(make([]byte, BTREE_PAGE_SIZE))
        }
        // link to the new tail node
        LNode(fl.set(fl.tailPage)).setNext(next)
        fl.tailPage = next
        // also add the head node if it's removed
        if head != 0 {
            LNode(fl.set(fl.tailPage)).setPtr(0, head)
            fl.tailSeq++
        }
    }
}
```

再次，空闲列表是自我管理的：在求助于追加之前，它将尝试从自身获取新尾节点。

## 7.4 带空闲列表的键值存储

### 页面管理

既然页面可以被重用，重用的页面将就地覆盖，因此使用映射来捕获挂起的更新。

```go
type KV struct {
    // ...
    page struct {
        flushed uint64            // database size in number of pages
        nappend uint64            // number of pages to be appended
        updates map[uint64][]byte // pending updates, including appended pages
    }
}
```

`BTree.new` 现在是 `KV.pageAlloc`，它在求助于追加之前使用空闲列表。

```go
// `BTree.new`, allocate a new page.
func (db *KV) pageAlloc(node []byte) uint64 {
    if ptr := db.free.PopHead(); ptr != 0 { // try the free list
        db.page.updates[ptr] = node
        return ptr
    }
    return db.pageAppend(node) // append
}
```

`KV.pageWrite` 返回一个可写页面副本以捕获就地更新。

```go
// `FreeList.set`, update an existing page.
func (db *KV) pageWrite(ptr uint64) []byte {
    if node, ok := db.page.updates[ptr]; ok {
        return node // pending update
    }
    node := make([]byte, BTREE_PAGE_SIZE)
    copy(node, db.pageReadFile(ptr)) // initialized from the file
    db.page.updates[ptr] = node
    return node
}
```

另一个变化是我们可能在更新页面之后再次读取它，因此 `KV.pageRead` 应首先咨询挂起的更新映射。

```go
// `BTree.get`, read a page.
func (db *KV) pageRead(ptr uint64) []byte {
    if node, ok := db.page.updates[ptr]; ok {
        return node // pending update
    }
    return db.pageReadFile(ptr)
}
func (db *KV) pageReadFile(ptr uint64) []byte {
    // same as `KV.pageRead` in the last chapter ...
}
```

### 更新元数据页面

元数据页面现在包括空闲列表指针（头和尾），它们与树根一起原子性地更新。

```go
| sig | root_ptr | page_used | head_page | head_seq | tail_page | tail_seq |
| 16B |    8B    |     8B    |     8B    |    8B    |     8B    |    8B    |
```

记住，空闲列表始终包含至少 1 个节点，我们在初始化空数据库时将分配一个空节点给它。

```go
func readRoot(db *KV, fileSize int64) error {
    if fileSize == 0 { // empty file
        // reserve 2 pages: the meta page and a free list node
        db.page.flushed = 2
        // add an initial node to the free list so it's never empty
        db.free.headPage = 1 // the 2nd page
        db.free.tailPage = 1
        return nil // the meta page will be written in the 1st update
    }
    // ...
}
```

由于 `headSeq` 被阻塞在 `maxSeq` 上，`maxSeq` 在更新之间更新为 `tailSeq` 以允许重用最后版本的页面。

```go
func updateFile(db *KV) error {
    // ...
    // prepare the free list for the next update
    db.free.SetMaxSeq()
    return nil
}
```

在本章中，我们仍然假设顺序访问。当我们稍后添加并发性时，`headSeq` 将被最早的读者阻塞。

## 7.5 键值存储的结论

我们所做的工作：

+   可写复制 B+ 树的文件布局。

+   使用 `fsync` 的持久性和原子性。

+   使用空闲列表管理磁盘页面。

这对于具有 `get`、`set`、`del` 的键值存储已经足够了。但在第二部分中还有更多：

+   在键值存储上的关系数据库。

+   并发事务。
