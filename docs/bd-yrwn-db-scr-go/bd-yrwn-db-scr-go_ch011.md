# 10. Secondary Indexes

## 10.1 作为额外键的二级索引

### 表模式

如第八章所述，二级索引只是包含主键的额外 KV 对。每个索引都通过 B+树中的键前缀来区分。

```go
type TableDef struct {
    // user defined
    Name    string
    Types   []uint32   // column types
    Cols    []string   // column names
    Indexes [][]string // the first index is the primary key
    // auto-assigned B-tree key prefixes for different tables and indexes
    Prefixes []uint32
}
```

第一个索引被用作主键，因为它也是一个索引。

### KV 结构

对于二级索引，我们可以将主键放在 B+树值中，用于查找完整行。然而，与主键不同，二级索引没有唯一约束，因此可以有重复的 B+树键。

我们可以不修改我们的 B+树以支持重复项，也可以将主键添加到 B+树键中，使其唯一，并留空值。

```go
create table t1 (
    k1 string,
    k2 int,
    v1 string,
    v2 string,
    primary key (k1, k2),
    index idx1 (v1),
    index idx2 (v2, k2)
);
```

|  | key | value |
| --- | --- | --- |
| t1 | prefix1, k1, k2 | v1, v2 |
| idx1 | prefix2, v1, k1, k2 | (empty) |
| idx2 | prefix3, v2, k2, k1 | (empty) |

## 10.2 使用二级索引

### 通过匹配列选择索引

要执行范围查询，我们必须选择与查询键匹配的索引，该索引存储在`Scanner`类型中，以便`Scanner.Deref()`可以使用它。

```go
type Scanner struct {
    // the range, from Key1 to Key2
    Cmp1 int // CMP_??
    Cmp2 int
    Key1 Record
    Key2 Record
    // internal
    db     *DB
    tdef   *TableDef
    index  int    // which index?
    iter   *BIter // the underlying B-tree iterator
    keyEnd []byte // the encoded Key2
}
```

索引可以是多个列。例如，索引(*a*, *b*)可以服务于查询(*a*, *b*) > (1, 2)。它也可以服务于查询*a* > 1，因为这与(*a*, *b*) > (1, +∞)相同。索引选择只是匹配列。

```go
func dbScan(db *DB, tdef *TableDef, req *Scanner) error {
    // ...
    isCovered := func(index []string) bool {
        key := req.Key1.Cols
        return len(index) >= len(key) && slices.Equal(index[:len(key)], key)
    }
    req.index = slices.IndexFunc(tdef.Indexes, isCovered)
    // ...
}
```

### 将缺失的列编码为无穷大

在最后一个例子中，使用索引(*a*, *b*)的查询*a* > 1 只使用了 1 个列，因此我们需要将剩余的列编码为无穷大。

| 输入查询 | 使用索引 |
| --- | --- |
| *a* > 1 | (*a*, *b*) > (1, +∞) |
| *a* ≤ 1 | (*a*, *b*) < (1, +∞) |
| *a* ≥ 1 | (*a*, *b*) > (1, −∞) |
| *a* < 1 | (*a*, *b*) < (1, −∞) |

这可以通过修改我们的有序编码来完成。首先，我们将`"\xff"`选择为+∞，`""`为-∞。由于没有列被编码为空字符串，我们可以在-∞情况下忽略缺失的列。在+∞情况下，我们将为每个编码列添加一个标签，这样它们就不会以`"\xff"`开头。

```go
// order-preserving encoding
func encodeValues(out []byte, vals []Value) []byte {
    for _, v := range vals {
        out = append(out, byte(v.Type)) // *added*: doesn't start with 0xff
        switch v.Type {
        case TYPE_INT64:
            var buf [8]byte
            u := uint64(v.I64) + (1 << 63)        // flip the sign bit
            binary.BigEndian.PutUint64(buf[:], u) // big endian
            out = append(out, buf[:]...)
        case TYPE_BYTES:
            out = append(out, escapeString(v.Str)...)
            out = append(out, 0) // null-terminated
        default:
            panic("what?")
        }
    }
    return out
}
```

我们将列类型代码作为标签添加到前面。这也使得调试更容易，因为我们现在可以通过查看十六进制转储来解码内容。

这是一个小步骤，用于支持前缀列上的范围查询。

```go
// for primary keys and indexes
func encodeKey(out []byte, prefix uint32, vals []Value) []byte {
    // 4-byte table prefix
    var buf [4]byte
    binary.BigEndian.PutUint32(buf[:], prefix)
    out = append(out, buf[:]...)
    // order-preserving encoded keys
    out = encodeValues(out, vals)
    return out
}

// for the input range, which can be a prefix of the index key.
func encodeKeyPartial(
    out []byte, prefix uint32, vals []Value, cmp int,
) []byte {
    out = encodeKey(out, prefix, vals)
    if cmp == CMP_GT || cmp == CMP_LE { // encode missing columns as infinity
        out = append(out, 0xff) // unreachable +infinity
    } // else: -infinity is the empty string
    return out
}
```

## 10.3 维护二级索引

### 与主数据同步

更新可能涉及多个带有二级索引的 B+树键。当一行被更改时，我们必须删除旧的索引键并插入新的键。为此，B+树接口被扩展以返回旧值。

```go
type UpdateReq struct {
    tree *BTree
    // out
    Added   bool   // added a new key
    Updated bool   // added a new key or an old key was changed
    Old     []byte // the value before the update
    // in
    Key  []byte
    Val  []byte
    Mode int
}
```

使用新信息：

```go
func dbUpdate(db *DB, tdef *TableDef, rec Record, mode int) (bool, error) {
    // ...
    // insert the row
    req := UpdateReq{Key: key, Val: val, Mode: mode}
    if _, err = db.kv.Update(&req); err != nil {
        return false, err
    }
    // maintain secondary indexes
    if req.Updated && !req.Added {
        // use `req.Old` to delete the old indexed keys ...
    }
    if req.Updated {
        // add the new indexed keys ...
    }
    return req.Updated, nil
}
```

### 多键更新的原子性

原子性不可组合！当涉及多个键时，我们失去了原子性，即使单个 KV 操作是原子的。如果数据库崩溃或在更新二级索引时发生错误，它应该回滚到之前的状态。

仅使用 get、set、del 来实现这一点很棘手，这就是为什么简单的 KV 接口非常有限。我们的下一步是一个*事务性*的 KV 接口，允许对多个键或甚至并发读取器执行原子操作。

## 10.4 KV 表和索引的总结

+   行和列作为 KVs。

+   范围查询。

    +   B+树迭代器。

    +   顺序保持编码。

+   二级索引。

    +   索引选择。

    +   需要一个事务性接口。
