# 14. 查询语言

## 14.1 表达式评估

`SELECT` 和 `UPDATE` 都包含需要评估的列表达式。

```go
type QLEvalContex struct {
    env Record // input row values
    out Value  // output
    err error
}
```

评估树与上一章讨论的一样明显。

```go
func qlEval(ctx *QLEvalContex, node QLNode) {
    switch node.Type {
    // refer to a column
    case QL_SYM:
        if v := ctx.env.Get(string(node.Str)); v != nil {
            ctx.out = *v
        } else {
            qlErr(ctx, "unknown column: %s", node.Str)
        }
    // a literal value
    case QL_I64, QL_STR:
        ctx.out = node.Value
    // operators
    case QL_NEG:
        qlEval(ctx, node.Kids[0])
        if ctx.out.Type == TYPE_INT64 {
            ctx.out.I64 = -ctx.out.I64
        } else {
            qlErr(ctx, "QL_NEG type error")
        }
    // ...
    }
}
```

`INSERT` 包含用空 `env` 评估的常量表达式。

## 14.2 范围查询

### 设置范围查询。

`SELECT`、`UPDATE` 和 `DELETE` 都可以进行范围查询，区别在于如何处理结果。`QLScan` 是表示范围查询的共同部分。

```go
type QLScan struct {
    Table  string // table name
    Key1   QLNode // index by
    Key2   QLNode
    Filter QLNode // filter
    Offset int64  // limit
    Limit  int64
}
```

它有 3 个阶段：`INDEX BY`、`LIMIT` 和 `FILTER`。`Scanner` 实现了 `INDEX BY`。

```go
func qlScanInit(req *QLScan, sc *Scanner) (err error) {
    // convert `QLNode` to `Record` and `CMP_??`
    if sc.Key1, sc.Cmp1, err = qlEvalScanKey(req.Key1); err != nil {
        return err
    }
    if sc.Key2, sc.Cmp2, err = qlEvalScanKey(req.Key2); err != nil {
        return err
    }
    switch { // special handling when `Key1` and `Key2` are not both present
    case req.Key1.Type == 0 && req.Key2.Type == 0: // no `INDEX BY`
        sc.Cmp1, sc.Cmp2 = CMP_GE, CMP_LE // full table scan
    case req.Key1.Type == QL_CMP_EQ && req.Key2.Type == 0:
        // equal by a prefix: INDEX BY key = val
        sc.Key2 = sc.Key1
        sc.Cmp1, sc.Cmp2 = CMP_GE, CMP_LE
    case req.Key1.Type != 0 && req.Key2.Type == 0:
        // open-ended range: INDEX BY key > val
        if sc.Cmp1 > 0 {
            sc.Cmp2 = CMP_LE // compare with a zero-length tuple
        } else {
            sc.Cmp2 = CMP_GE
        }
    }
    return nil
}
```

### 重新审视无穷编码

`INDEX BY` 采用上一章中定义的 3 种形式之一。

1.  `a > start AND a < end`：一个区间 (*s**t**a**r**t*, *e**n**d*).

1.  `a > s`：一个开区间 (*s*, +∞)。

1.  `a = p`：索引的前缀。

假设索引是 (*a*, *b*)。使用索引前缀的查询已经在第十章中的键编码中处理了。所以……

+   `a = p` 等价于 `a >= p AND a <= p`，编码为 (*a*, *b*) ≥ (*p*, −∞) 和 (*a*, *b*) ≤ (*p*, +∞)。

+   `a > s` 等价于 `a > s AND () <= ()`，编码为 (*a*, *b*) > (*s*, −∞) 和 (*a*,) < (+∞,).

由于使用了空元组 `()`, `Key1` 和 `Key2` 现在可以有不同的列集，因此我们必须修改索引选择以允许这样做。

```go
func dbScan(tx *DBTX, tdef *TableDef, req *Scanner) error {
    // ...
    covered := func(key []string, index []string) bool {
        return len(index) >= len(key) && slices.Equal(index[:len(key)], key)
    }
    req.index = slices.IndexFunc(tdef.Indexes, func(index []string) bool {
        return covered(req.Key1.Cols, index) && covered(req.Key2.Cols, index)
    })
    // ...
}
```

## 14.3 结果迭代器

### 一直到底的迭代器

下一个阶段是 `LIMIT` 和 `FILTER`。结果从迭代器中消费。

```go
type RecordIter interface {
    Valid() bool
    Next()
    Deref(*Record) error
}
```

为什么使用迭代器而不是结果数组？因为数据库可以潜在地处理大于内存的数据，迭代器不需要一次性将结果准备好在内存中，它甚至可以在结果产生时流式传输结果。

`SELECT` 的迭代器链。

| 迭代器 | 输出 | 角色 |
| --- | --- | --- |
| `BIter` | KV | 遍历 B+ 树。 |
| `KVIter` | KV | 将快照与本地更新合并。 |
| `Scanner` | 行 | 解码记录并跟随二级索引。 |
| `qlScanIter` | 行 | 偏移量、限制和过滤行。 |
| `qlSelectIter` | 行 | 在 `SELECT` 中评估表达式。 |

### 使用迭代器转换数据

迭代器接受另一个迭代器作为输入以转换项目流。这是一种有用的编程模式。

```go
type qlSelectIter struct {
    iter  RecordIter    // input
    names []string
    exprs []QLNode
}

func (iter *qlSelectIter) Valid() bool {
    return iter.iter.Valid()
}
func (iter *qlSelectIter) Next() {
    iter.iter.Next()
}
func (iter *qlSelectIter) Deref(rec *Record) error {
    if err := iter.iter.Deref(rec); err != nil {
        return err
    }
    vals, err := qlEvelMulti(*rec, iter.exprs)
    if err != nil {
        return err
    }
    *rec = Record{iter.names, vals}
    return nil
}
```

`qlScanIter` 稍微复杂一些，因为过滤需要一些记录。

```go
type qlScanIter struct {
    // input
    req *QLScan
    sc  Scanner
    // state
    idx int64
    end bool
    // cached output item
    rec Record
    err error
}
```

## 14.4 结论和下一步

我们有多个接口到持久性、事务性数据库：

1.  可以嵌入到应用程序中的键值对。

1.  可以嵌入到应用程序中的关系型数据库。

1.  关系型数据库的类似 SQL 的查询语言。

不添加新功能，我们可以创建一个 *网络协议* 来允许数据库在不同的进程或机器上运行。Go 中的网络编程是高级且简单的，但你可以始终以“从头开始”的态度学习更多，这在 [“自己动手构建 Redis”](https://build-your-own.org/redis/) 一书中可以找到。

由于我们已经有了一个基本的解析器和解释器，我们可以着手处理编译器。你可以创建一种编程语言，并将其编译成机器码，而不是仅仅进行解释。参见[“从源代码到机器码”](https://build-your-own.org/compiler/)这本书。
