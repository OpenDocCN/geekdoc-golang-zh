# 08. 基于 KV 的表

## 8.1 将行编码为 KVs

### 索引查询：点查询和范围查询

在关系型数据库中，数据被建模为包含行和列的二维表。用户通过 SQL 表达他们的*意图*，数据库神奇地返回结果。不那么神奇的是，虽然数据库可以执行任意查询，但并非所有查询在 OLTP 工作负载中都是实际可行的（高效且可扩展），OLTP 总是要求用户通过适当的模式设计来控制查询的执行方式。

索引查询的执行过程归结为两个操作：

1.  点查询：通过给定的键查找一行。

1.  范围查询：通过范围查找行；按排序顺序迭代结果。

那就是为什么 B+树和 LSM 树被考虑，而哈希表则不被考虑。

### 主键作为“键”

让我们先考虑点查询。要找到一行，必须有唯一标识该行的方法，那就是主键，它是列的一个子集。

```go
create table t1 (
    k1 string,
    k2 int,
    v1 string,
    v2 string,
    primary key (k1, k2)
);
```

直观地，主键列放在“键”中，其余的放在“值”中。

|  | 键 | 值 |
| --- | --- | --- |
| t1 | k1, k2 | v1, v2 |

一些数据库允许没有主键的表，它们所做的是添加一个隐藏的、自动生成的主键。

### 作为独立表的二级索引

除了主键之外，一个表可以通过多种方式索引。这是通过额外的间接引用解决的：二级索引。

```go
create table t1 (
    k1 string,
    k2 int,
    v1 string,
    v2 string,
    primary key (k1, k2),
    index idx1 (v1),
    index idx2 (v2, v1)
);
```

逻辑上，每个索引就像一个独立的表：

```go
create table idx1 (
    -- indexed key (v1)
    v1 string,
    -- primary key (k1, k2)
    k1 string,
    k2 int
);
create table idx2 (
    -- indexed key (v2, v1)
    v2 string,
    v1 string,
    -- primary key (k1, k2)
    k1 string,
    k2 int
);
```

这会增加一个额外的键来查找唯一的行标识符（主键）。

|  | 键 | 值 |
| --- | --- | --- |
| t1 | k1, k2 | v1, v2 |
| idx1 | v1 | k1, k2 |
| idx2 | v2, v1 | k1, k2 |

主键也是一个索引，但具有唯一约束。

### 替代方案：自动生成的行 ID

一些数据库使用自动生成的 ID 作为“真正的”主键，而不是用户选择的主键。在这种情况下，主键和二级索引之间没有区别；用户主键也是一个间接引用。

|  | 键 | 值 |
| --- | --- | --- |
| t1 | ID | k1, k2, v1, v2 |
| 主键 | k1, k2 | ID |
| idx1 | v1 | ID |
| idx2 | v2, v1 | ID |

优点是自动生成的 ID 可以是一个小的、固定宽度的整数，而用户主键可以任意长。这意味着……

+   对于 ID 键，内部节点可以存储更多的键（更短的树）。

+   二级索引较小，因为它们不重复用户主键。

## 8.2 数据库模式

### 表前缀

一个数据库可以包含多个表和索引。我们将为键添加一个自动生成的前缀，以便它们可以共享一个单一的 B+树。这比维护多个树要少做很多工作。

|  | 键 | 值 |
| --- | --- | --- |
| table1 | prefix1 + 列… | 列… |
| table2 | prefix2 + 列… | 列… |
| index1 | prefix3 + 列… | 列… |

前缀是一个 32 位的自增整数，你也可以用表名代替，但缺点是它可以任意长。

### 数据类型

关系型数据库相对于键值数据库的一个优点是它们支持更多的数据类型。为了反映这一方面，我们将支持两种数据类型：字符串和整数。

```go
const (
    TYPE_BYTES = 1 // string (of arbitrary bytes)
    TYPE_INT64 = 2 // integer; 64-bit signed
)

// table cell
type Value struct {
    Type uint32 // tagged union
    I64  int64
    Str  []byte
}
```

单元格`Value`是特定类型的标记联合。

### 记录

`Record`表示列名和值的列表。

```go
// table row
type Record struct {
    Cols []string
    Vals []Value
}

func (rec *Record) AddStr(col string, val []byte) *Record {
    rec.Cols = append(rec.Cols, col)
    rec.Vals = append(rec.Vals, Value{Type: TYPE_BYTES, Str: val})
    return rec
}
func (rec *Record) AddInt64(col string, val int64) *Record
func (rec *Record) Get(col string) *Value
```

### 模式

在本章中，我们只考虑主键，将索引留到以后。

```go
type TableDef struct {
    // user defined
    Name  string
    Types []uint32 // column types
    Cols  []string // column names
    PKeys int      // the first `PKeys` columns are the primary key
    // auto-assigned B-tree key prefixes for different tables
    Prefix uint32
}
```

### 内部表

表模式存储在哪里？因为我们正在编写数据库，我们知道如何存储东西；我们将它们存储在预定义的内部表中。

```go
var TDEF_TABLE = &TableDef{
    Prefix: 2,
    Name:   "@table",
    Types:  []uint32{TYPE_BYTES, TYPE_BYTES},
    Cols:   []string{"name", "def"},
    PKeys:  1,
}
```

`def`列是 JSON 序列化的`TableDef`。这就像：

```go
create table `@table` (
    `name` string,  -- table name
    `def` string,   -- schema
    primary key (`name`)
);
```

我们还需要保留一些额外信息，例如用于生成表前缀的自动递增计数器。让我们为此定义另一个内部表。

```go
var TDEF_META = &TableDef{
    Prefix: 1,
    Name:   "@meta",
    Types:  []uint32{TYPE_BYTES, TYPE_BYTES},
    Cols:   []string{"key", "val"},
    PKeys:  1,
}
```

## 8.3 获取、更新、插入、删除、创建

### 点查询和更新接口

读取和写入单行的接口：

```go
func (db *DB) Get(table string, rec *Record) (bool, error)
func (db *DB) Insert(table string, rec Record) (bool, error)
func (db *DB) Update(table string, rec Record) (bool, error)
func (db *DB) Upsert(table string, rec Record) (bool, error)
func (db *DB) Delete(table string, rec Record) (bool, error)
```

`DB`是`KV`的包装器：

```go
type DB struct {
    Path string
    kv KV
}
```

### 通过主键查询

`rec`参数是输入主键。它也是输出行。

```go
// get a single row by the primary key
func dbGet(db *DB, tdef *TableDef, rec *Record) (bool, error) {
    // 1\. reorder the input columns according to the schema
    values, err := checkRecord(tdef, *rec, tdef.PKeys)
    if err != nil {
        return false, err
    }
    // 2\. encode the primary key
    key := encodeKey(nil, tdef.Prefix, values[:tdef.PKeys])
    // 3\. query the KV store
    val, ok := db.kv.Get(key)
    if !ok {
        return false, nil
    }
    // 4\. decode the value into columns
    for i := tdef.PKeys; i < len(tdef.Cols); i++ {
        values[i].Type = tdef.Types[i]
    }
    decodeValues(val, values[tdef.PKeys:])
    rec.Cols = tdef.Cols
    rec.Vals = values
    return true, nil
}
```

处理列的代码只是平凡的，我们将跳过它。

```go
// reorder a record and check for missing columns.
// n == tdef.PKeys: record is exactly a primary key
// n == len(tdef.Cols): record contains all columns
func checkRecord(tdef *TableDef, rec Record, n int) ([]Value, error)
```

下一步是编码和解码，可以是任何序列化方案。

```go
// encode columns for the "key" of the KV
func encodeKey(out []byte, prefix uint32, vals []Value) []byte
// decode columns from the "value" of the KV
func decodeValues(in []byte, out []Value)
```

### 读取模式

用户接口通过名称引用表，因此我们必须首先获取其模式。

```go
// get a single row by the primary key
func (db *DB) Get(table string, rec *Record) (bool, error) {
    tdef := getTableDef(db, table)
    if tdef == nil {
        return false, fmt.Errorf("table not found: %s", table)
    }
    return dbGet(db, tdef, rec)
}
```

这只是一个查询内部表`@table`的 JSON 编码的`TableDef`。

```go
func getTableDef(db *DB, name string) *TableDef {
    rec := (&Record{}).AddStr("name", []byte(name))
    ok, err := dbGet(db, TDEF_TABLE, rec)
    assert(err == nil)
    if !ok {
        return nil
    }

    tdef := &TableDef{}
    err = json.Unmarshal(rec.Get("def").Str, tdef)
    assert(err == nil)
    return tdef
}
```

我们可以在内存中缓存表模式以减少查询次数，因为没有任何理智的应用程序需要大量的表。

### 插入或更新一行

有 3 个 SQL 更新语句，它们在处理现有行的方式上有所不同：

+   `INSERT`仅添加新行（由主键标识）。

+   `UPDATE`仅修改现有行。

+   `UPSERT`添加新行或修改现有行。

（注意：`UPSERT`是 PostgreSQL 特定的。在 MySQL 中它是`ON DUPLICATED KEY UPDATE`。在 SQLite 中它是`INSERT OR REPLACE`）。

这通过扩展`BTree.Insert`并使用模式标志来实现。

```go
// update modes
const (
    MODE_UPSERT      = 0 // insert or replace
    MODE_UPDATE_ONLY = 1 // update existing keys
    MODE_INSERT_ONLY = 2 // only add new keys
)

type UpdateReq struct {
    tree *BTree
    // out
    Added   bool   // added a new key
    // in
    Key  []byte
    Val  []byte
    Mode int
}

func (tree *BTree) Update(req *UpdateReq)
```

此处的更新函数仅处理完整行。部分更新（读取-修改-写入）在更高级别（查询语言）中实现。

```go
func dbUpdate(db *DB, tdef *TableDef, rec Record, mode int) (bool, error) {
    values, err := checkRecord(tdef, rec, len(tdef.Cols))
    if err != nil {
        return false, err
    }
    key := encodeKey(nil, tdef.Prefix, values[:tdef.PKeys])
    val := encodeValues(nil, values[tdef.PKeys:])
    return db.kv.Update(key, val, mode)
}
```

### 创建一个表

创建表的过程相当无聊：

1.  读取`@table`以检查重复的名称。

1.  从`@meta`读取表前缀计数器。

1.  在`@meta`中增加和更新表前缀计数器。

1.  将模式插入到`@table`。

```go
func (db *DB) TableNew(tdef *TableDef) error
```

此过程涉及更新 2 个键，所以我们在这里失去了原子性。这将在我们添加事务时得到解决。

## 8.4 KV 上表的结论

在 KV 上的表本质上没有区别，它只是数据序列化和保留模式的一些额外步骤。然而，工作还没有完成。下一步是：

+   范围查询。

+   二级索引。
