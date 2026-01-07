# 11. 原子事务

## 11.1 全或无效应

上一章的二级索引需要原子多键更新。这不仅对于内部数据库一致性是必要的，而且对于应用程序数据一致性也是有用的，想想账户余额与账户交易。

我们将取消 get-set-del 接口，并添加一个新的接口以允许原子执行一组操作。并发将在下一章讨论。

### 提交和回滚

我们将添加接口来标记事务的开始和结束。在结束时，由于错误或用户请求（`Abort`），更新要么生效（提交）要么被丢弃（回滚）。

```go
// begin a transaction
func (kv *KV) Begin(tx *KVTX)
// end a transaction: commit updates; rollback on error
func (kv *KV) Commit(tx *KVTX) error
// end a transaction: rollback
func (kv *KV) Abort(tx *KVTX)
```

### 通过写时复制实现原子性

使用写时复制，提交和回滚只是更新根指针。这已经在第六章中作为错误处理实现。

```go
type KVTX struct {
    db   *KV
    meta []byte // for the rollback
}

func (kv *KV) Begin(tx *KVTX) {
    tx.db = kv
    tx.meta = saveMeta(tx.db)
}
func (kv *KV) Commit(tx *KVTX) error {
    return updateOrRevert(tx.db, tx.meta)
}
func (kv *KV) Abort(tx *KVTX) {
    // nothing has written, just revert the in-memory states
    loadMeta(tx.db, tx.meta)
    // discard temporaries
    tx.db.page.nappend = 0
    tx.db.page.updates = map[uint64][]byte{}
}
```

之前，`updateOrRevert()`在单个键更新后被调用。现在它被移动到`KVTX.Commit()`。B+树可以更新所需次数，重要的是根指针。

```go
// previous chapter!!!
func (db *KV) Update(req *UpdateReq) (bool, error) {
    meta := saveMeta(db)
    if !db.tree.Update(req) {
        return false, nil
    }
    err := updateOrRevert(db, meta)
    return err == nil, err
}
```

### 替代方案：通过日志实现原子性

在写时复制的树中，更新由根指针捕获，而不是需要日志来捕获更新的原地更新。

日志用于在事务被中止时回滚更新。问题是 IO 错误阻止了进一步的更新，因此回滚留给恢复机制，这在写时复制（见`updateOrRevert`）中也同样适用。

更新一旦在日志中`fsync`后就被认为是持久的。因此，数据库可以在仅进行一次`fsync`后向客户端返回成功，只要日志被认为用于查询，并最终合并到主数据存储中。

## 11.2 事务性接口

### 将树操作移动到事务中

树操作现在与事务相关联，因此它们被移动到`KVTX`。

```go
func (tx *KVTX) Seek(key []byte, cmp int) *BIter {
    return tx.db.tree.Seek(key, cmp)
}
func (tx *KVTX) Update(req *UpdateReq) bool {
    return tx.db.tree.Update(req)
}
func (tx *KVTX) Del(req *DeleteReq) bool {
    return tx.db.tree.Delete(req)
}
```

注意，这些函数不再返回错误，因为实际的磁盘更新被移动到`KVTX.Commit()`。

### 事务性表操作

对于基于表的接口，只需向`KVTX`添加一个包装器类型。

```go
type DBTX struct {
    kv KVTX
    db *DB
}

func (db *DB) Begin(tx *DBTX)
func (db *DB) Commit(tx *DBTX) error
func (db *DB) Abort(tx *DBTX)
```

将表操作移动到那个包装器中。

```go
func (tx *DBTX) Scan(table string, req *Scanner) error
func (tx *DBTX) Set(table string, rec Record, mode int) (bool, error)
func (tx *DBTX) Delete(table string, rec Record) (bool, error)
```

这些操作不再处理 IO 错误，因此没有更新二级索引的错误处理。

## 11.3 可选优化

一个工作关系数据库是一个重要的里程碑，尽管它只支持顺序操作。对于进一步的挑战，有一些优化可以考虑。

### 减少多键更新的复制

写时复制在单个更新中将节点从叶节点复制到根节点，这对于多键更新来说是不太理想的，因为中间树中的节点在事务中分配、更新一次然后被删除。

优化是在事务中只复制一次节点，并在复制的节点上使用原地更新。

### 范围删除

尽管我们现在可以进行多键更新，但删除大量键，例如删除表，在资源使用方面仍然存在问题。删除表的一种简单方法是迭代并逐个删除键。这会将整个表读入内存，并在删除前反复更新节点，从而进行无用的工作。

一些数据库为每个表使用单独的文件，因此这不是问题。在我们的情况下，我们使用单个 B+树来处理所有内容，因此我们可以实现一个*范围删除*操作，该操作释放了具有范围的全部叶节点，甚至不需要查看它们。

### 压缩公共前缀

在任何排序数据中，相邻键很可能共享一个公共前缀。在典型的关系型数据库使用中，多列键也会导致共享前缀。因此，在节点内压缩键存在一个机会。

前缀压缩使得实现变得更加困难（有趣），尤其是在节点大小不易预测合并和拆分时。
