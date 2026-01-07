# 12. 并发控制

## 12.1 并发级别

### 问题：交错读取者和写入者

并发客户端可以随意进入和退出事务，在之间请求读取和写入。为了简化分析，让我们假设进入/退出/读取/写入是原子步骤，因此并发 TXs 只是交错步骤。

我们还将区分只读 TXs 和读写 TXs。这是因为……

+   并发读取者比并发写入者的问题容易得多。

+   许多应用程序是读密集型，其中读取性能更重要。

### 读取-写入锁 (RWLock)

如果不知道如何进行并发，你总是可以添加互斥锁（锁）来序列化任何数据访问。对于读取性能，你可以使用*读取-写入锁*。它允许多个并发读取者，但只有一个写入者。

+   如果没有写入者，则无法进行更改，并发读取者是允许的。

+   当写入者想要进入时，它会等待所有读取者都离开。

+   读取者被写入者阻塞，但不会被其他读取者阻塞。

这种方法的实用性有限；写入者之间没有并发，长时间运行的 TXs 不好，因为读取者和写入者会相互阻塞。

### 读取-复制-更新 (RCU)

为了防止读取者和写入者相互阻塞，我们可以让读取者和写入者各自在自己的数据版本上工作。

+   有一个指向*不可变*数据的指针，读取者只需将其作为快照抓取。

+   单个写入者更新自己的*副本*，然后翻转指向它的指针。

由于我们是写时复制，我们免费获得这种级别的并发。但单个写入者仍然不足，因为 TX 的生命周期由客户端控制，可以是任意长的。

### 乐观并发控制

并发写入者会导致冲突，例如：

| Seq | TX1 | TX2 |
| --- | --- | --- |
| 1 | 读取 `a` |  |
| 2 |  | 写入 `a := 1` |
| 3 | 写入 `b := a` |  |
| 4 | 提交 |  |
| 5 |  | 提交 |

TX1 依赖于 TX2 修改的相同键，因此它们不能同时成功。

注意，一些看似“只写”的操作实际上有读取依赖。例如，我们的更新/删除接口报告键是否被更新/删除，这取决于键的先前状态。因此，以下场景也是一个冲突。

| Seq | TX1 | TX2 |
| --- | --- | --- |
| 1 | 写入 `a := 1` |  |
| 2 |  | 删除 `a` |
| 3 | 提交 |  |
| 4 |  | 提交 |

处理冲突的一种方法是在检测到冲突时简单地中止 TX。

1.  TX 开始。

1.  读取在快照上，但写入在本地缓冲。

1.  在提交之前，验证是否存在与已提交 TXs 的冲突。

1.  TX 结束。

    +   如果发生冲突，则中止并回滚。

    +   否则，将缓冲的写入操作转移到数据库。

注意，*验证和提交*是一个原子步骤。这被称为*乐观*并发控制，因为它假设冲突很少，并没有采取措施来防止它们。我们将实现这个方案，但还有其他替代方案需要了解。

### 替代方案：悲观并发控制

在乐观并发控制中，如果发生冲突，事务无法进行，这对应用程序的视角来说并不很有帮助，因为它们所能做的就是循环重试。处理冲突的另一种方式是通过锁定来防止它们。事务将对其依赖项获取锁，这样可能冲突的事务将相互等待。

这听起来要好得多，尤其是在最后一个例子中，写入/删除操作可以无问题地进行。然而，这仍然不能保证进度，因为事务（TXs）现在可能会因为**死锁**而失败。

死锁是当两个当事人都在等待对方释放他们拥有的（不同的）锁时。只要依赖关系图中存在循环，这种情况也会发生在两个以上当事人之间。在并发编程中，应该按照预定义的顺序获取锁以避免循环。对于数据库来说这不是这种情况，因为客户端可以以任何顺序获取锁，所以数据库必须检测和解决死锁，这是一个图问题。

### 并发控制的比较

|  | 读取器-读取器 | 读取器-写入器 | 写入器-写入器 | 冲突 |
| --- | --- | --- | --- | --- |
| RWLock | 通过 | 阻塞 | 阻塞 | - |
| RCU | 通过 | 通过 | 阻塞 | - |
| 乐观锁 | 通过 | 通过 | 通过 | 中断 |
| 悲观锁 | 通过 | 锁定 | 锁定 | 防止 |

## 12.2 读取者的快照隔离

隔离级别指的是事务如何看待其他事务的更改。在写时复制（copy-on-write）的情况下这不是一个问题，因为事务是在 B+树的**快照**上操作的。

### 捕获本地更新

事务保留 DB 的快照和本地更新。

+   快照只是一个带有写时复制的根指针。

+   本地更新保存在内存中的 B+树中。

```go
type KVTX struct {
    // a read-only snapshot
    snapshot BTree
    // captured KV updates:
    // values are prefixed by a 1-byte flag to indicate deleted keys.
    pending BTree
    // ...
}
```

两个树在事务开始时初始化。

```go
// begin a transaction
func (kv *KV) Begin(tx *KVTX) {
    // read-only snapshot, just the tree root and the page read callback
    tx.snapshot.root = kv.tree.root
    tx.snapshot.get = ... // read from mmap'ed pages ...
    // in-memory tree to capture updates
    pages := [][]byte(nil)
    tx.pending.get = func(ptr uint64) []byte { return pages[ptr-1] }
    tx.pending.new = func(node []byte) uint64 {
        pages = append(pages, node)
        return uint64(len(pages))
    }
    tx.pending.del = func(uint64) {}
}
```

为了表示已删除的键，`KVTX.pending`中的值前面有一个 1 字节的标志。

```go
    FLAG_DELETED = byte(1)
    FLAG_UPDATED = byte(2)
```

### 读取自己的写入

在事务（TX）内部，客户端应该能够读取它刚刚写入的内容，即使它尚未提交，因此查询应该在`KVTX.snapshot`之前咨询`KVTX.pending`。这就是为什么写入被保存在 B+树中而不是仅仅是一个列表。

```go
// point query. combines captured updates with the snapshot
func (tx *KVTX) Get(key []byte) ([]byte, bool) {
    val, ok := tx.pending.Get(key)
    switch {
    case ok && val[0] == FLAG_UPDATED: // updated in this TX
        return val[1:], true
    case ok && val[0] == FLAG_DELETED: // deleted in this TX
        return nil, false
    case !ok: // read from the snapshot
        return tx.snapshot.Get(key)
    default:
        panic("unreachable")
    }
}
```

对于范围查询，添加了一个新的迭代器类型来结合两个树。

```go
// an iterator that combines pending updates and the snapshot
type CombinedIter struct {
    top *BIter // KVTX.pending
    bot *BIter // KVTX.snapshot
    // ...
}
```

### 自由列表中的版本号

由于读取者可以持有 DB 的旧版本，自由列表不能放弃这些版本的页面。我们将通过为每个版本分配单调递增的版本号来解决这个问题。这也可以称为一个**时间戳**（逻辑上）。

+   我们跟踪进行中的事务及其基于的版本号。

+   每个添加到自由列表的页面都与版本号相关联。

+   列表永远不会放弃比最老事务更新的页面。

这通过在从列表头部消费时检查版本号来实现。记住，自由列表是一个 FILO（先进后出），所以最老版本的页面将首先被消费。

修改 1：`KVTX`和`KV`中的版本号

```go
type KVTX struct {
    // a read-only snapshot
    snapshot BTree
    version  uint64 // based on KV.version
    // ...
}
type KV struct {
    // ...
    version uint64    // monotonic version number; persisted in the meta page
    ongoing []uint64  // version numbers of concurrent TXs
}
```

修改 2：自由列表增强。

```go
// | next | pointer + version | unused |
// |  8B  |     n*(8B+8B)     |   ...  |
type FreeList struct {
    // ...
    maxSeq uint64 // saved `tailSeq` to prevent consuming newly added items
    maxVer uint64 // the oldest reader version
    curVer uint64 // version number when committing
}
```

+   当事务退出时，`maxVer` 被维护为 `KV.ongoing` 中的最老版本。除了现有的 `maxSeq`，它还防止页面重用。

+   在提交时，`curVar` 被作者设置为下一个版本。

## 12.3 处理作者间的冲突

### 检测与历史记录的冲突

修改 1：所有读取操作都添加到 `KVTX.reads`（指针和范围查询）中。

```go
// start <= key <= stop
type KeyRange struct {
    start []byte
    stop  []byte
}
type KVTX struct {
    // ...
    reads []KeyRange
}
func (tx *KVTX) Get(key []byte) ([]byte, bool) {
    tx.reads = append(tx.reads, KeyRange{key, key}) // dependency
    // ...
}
```

修改 2：每个成功的提交都添加到 `KV.history` 中。

```go
type KV struct {
    // ...
    history []CommittedTX // chanages keys; for detecting conflicts
}
type CommittedTX struct {
    version uint64
    writes  []KeyRange // sorted
}
func (kv *KV) Commit(tx *KVTX) error {
    // ...
    if len(writes) > 0 {
        kv.history = append(kv.history, CommittedTX{kv.version, writes})
    }
    return nil
}
```

冲突检测通过检查其依赖与比其基础版本更新的历史之间的重叠来工作。

```go
func detectConflicts(kv *KV, tx *KVTX) bool {
    for i := len(kv.history) - 1; i >= 0; i-- {
        if !versionBefore(tx.version, kv.history[i].version) {
            break // sorted
        }
        if rangesOverlap(tx.reads, kv.history[i].writes) {
            return true
        }
    }
    return false
}
```

当最老的事务退出时，历史将被裁剪。

### 序列化内部数据结构

在分析中，事务（TX）被简化为交错步骤，实际上，这些步骤可以在并行线程中运行，因此应该按顺序执行，因为它们共享 `KV` 结构。

```go
type KV struct {
    // ...
    mutex   sync.Mutex    // serialize TX methods
}
func (kv *KV) Begin(tx *KVTX) {
    kv.mutex.Lock()
    defer kv.mutex.Unlock()
    // ...
}
func (kv *KV) Commit(tx *KVTX)  // same
func (kv *KV) Abort(tx *KVTX)   // same
```

我们可以使用这个锁来处理所有 `KVTX` 方法。但有一些方法可以减少锁定。例如，我们不需要序列化读写方法，因为……

+   写操作仅在 `KVTX.pending` 上工作，它们永远不会触及 `KV`。

+   读取仅触及 `KV.mmap.chunks`，这是 `mmap` 返回的片段。

提交步骤可能会通过追加修改 `KV.mmap.chunks`，因此我们将为每个事务（TX）使用一个本地副本。这个片段是只追加的，所以浅拷贝就足够了。

```go
func (kv *KV) Begin(tx *KVTX) {
    kv.mutex.Lock()
    defer kv.mutex.Unlock()
    // read-only snapshot, just the tree root and the page read callback
    tx.snapshot.root = kv.tree.root
    chunks := kv.mmap.chunks // copied to avoid updates from writers
    tx.snapshot.get = func(ptr uint64) []byte { return mmapRead(ptr, chunks) }
    // ...
}
```

这样，读写方法不需要锁，并且可以并行运行。这很好，因为读取可能会触发页面错误并阻塞线程。

到目前为止，只有 `Begin`、`Commit` 和 `Abort` 被序列化。但考虑到 `Commit` 涉及 I/O，我们可以在等待 I/O 的同时释放锁，以允许其他事务进入并允许只读事务退出。提交步骤应该通过另一个锁与其他提交步骤一起序列化。这留作练习。
