# 09. 范围查询

## 9.1 B+树迭代器

### 迭代器接口

基本操作是范围查询的*seek*和*iterate*。B+树的位置由状态迭代器`BIter`表示。

```go
// find the closest position that is less or equal to the input key
func (tree *BTree) SeekLE(key []byte) *BIter

// get the current KV pair
func (iter *BIter) Deref() ([]byte, []byte)
// precondition of the Deref()
func (iter *BIter) Valid() bool
// moving backward and forward
func (iter *BIter) Prev()
func (iter *BIter) Next()
```

例如，查询`a <= key`看起来像这样：

```go
for iter := tree.SeekLE(key); iter.Valid(); iter.Prev() {
    k, v := iter.Deref()
    // ...
}
```

### 导航树

需要当前位置来在节点内找到其兄弟键。如果兄弟键在兄弟节点中，我们需要回溯到父节点。由于我们不使用父指针，我们需要从根到叶子的整个路径。

```go
type BIter struct {
    tree *BTree
    path []BNode  // from root to leaf
    pos  []uint16 // indexes into nodes
}
```

移动迭代器就像在逐位增加数字时携带一样。

```go
func (iter *BIter) Next() {
    iterNext(iter, len(iter.path)-1)
}
func iterNext(iter *BIter, level int) {
    if iter.pos[level]+1 < iter.path[level].nkeys() {
        iter.pos[level]++ // move within this node
    } else if level > 0 {
        iterNext(iter, level-1) // move to a sibling node
    } else {
        iter.pos[len(iter.pos)-1]++ // past the last key
        return
    }
    if level+1 < len(iter.pos) { // update the child node
        node := iter.path[level]
        kid := BNode(iter.tree.get(node.getPtr(iter.pos[level])))
        iter.path[level+1] = kid
        iter.pos[level+1] = 0
    }
}
```

### 寻找键

寻找键就像是一个点查询，记录了路径。

```go
// find the closest position that is less or equal to the input key
func (tree *BTree) SeekLE(key []byte) *BIter {
    iter := &BIter{tree: tree}
    for ptr := tree.root; ptr != 0; {
        node := tree.get(ptr)
        idx := nodeLookupLE(node, key)
        iter.path = append(iter.path, node)
        iter.pos = append(iter.pos, idx)
        ptr = node.getPtr(idx)
    }
    return iter
}
```

`nodeLookupLE`用于小于等于，你还需要其他运算符。

```go
const (
    CMP_GE = +3 // >=
    CMP_GT = +2 // >
    CMP_LT = -2 // <
    CMP_LE = -3 // <=
)
func (tree *BTree) Seek(key []byte, cmp int) *BIter
```

## 9.2 顺序保持编码

### 将任意数据按字节字符串排序

我们的 B+树处理任意字节的字符串键。但列可以是其他类型，例如数字，键也可以是多列。为了支持范围查询，序列化键必须根据其数据类型进行比较。

显然的方法是将`bytes.Compare`替换为一个回调，该回调根据表模式解码和比较键。

另一种方法是**选择一种特殊的序列化格式，使得生成的字节反映排序顺序**。这是我们将会采取的捷径。

### 数字

让我们从简单的问题开始：如何编码*无符号整数*，以便它们可以通过`bytes.Compare`进行比较？`bytes.Compare`按字节逐个比较，直到遇到差异。因此，在比较中，第一个字节是最重要的，如果我们把整数的最高位（较高的位）放在前面，它们就可以按字节比较。这正好是大端整数。

```go
0x0000000000000001 -> 00 00 00 00 00 00 00 01
0x0000000000000002 -> 00 00 00 00 00 00 00 02
                  ...
0x00000000000000ff -> 00 00 00 00 00 00 00 ff
0x0000000000000100 -> 00 00 00 00 00 00 01 00
```

接下来，我们将考虑*有符号整数*，它们由[二进制补码](https://en.wikipedia.org/wiki/Signed_number_representations#Two's_complement)表示。在二进制补码表示中，无符号值的上半部分简单地偏移到负值。为了确保正确的顺序，正半部分与负半部分交换，这正好是翻转最高位。

```go
            var buf [8]byte
            u := uint64(v.I64) + (1 << 63)        // flip the sign bit
            binary.BigEndian.PutUint64(buf[:], u) // big endian
```

一些示例：

| `int64` | 编码的字节 |
| --- | --- |
| `MinInt64` | `00 00 00 00 00 00 00 00` |
| -2 | `7f ff ff ff ff ff ff fe` |
| -1 | `7f ff ff ff ff ff ff ff` |
| 0 | `80 00 00 00 00 00 00 00` |
| 1 | `80 00 00 00 00 00 00 01` |
| `MaxInt64` | `ff ff ff ff ff ff ff ff` |

因此，一般思路是：

+   将位按顺序排列，使得更重要的位先出现（大端序）。

+   按正确顺序将位重新映射到无符号整数。

读者练习：将此应用于浮点数（符号 + 幅度 + 指数）。

### 字符串

键可以是多列。但`bytes.Compare`只能与单个字符串列一起使用，因为它需要长度。我们不能简单地将字符串列连接起来，因为这会创建歧义。例如，`("a", "bc")`与`("ab", "c")`。

有两种方式可以编码带长度的字符串，一种是在前面添加长度，这需要解码。另一种是在末尾放置一个分隔符，例如空字节。前面的例子编码为`"a\x00bc\x00"`和`"ab\x00c\x00"`。

分隔符的问题在于输入不能包含分隔符，这个问题通过*转义*分隔符来解决。我们将使用字节 0x01 作为转义字节，而转义字节本身也必须被转义。因此，我们需要进行 2 次转换：

```go
00 -> 01 01
01 -> 01 02
```

注意，转义序列仍然保留了排序顺序。

### 元组

多列比较（元组）是逐列进行的，直到遇到差异。这就像字符串比较一样，只不过每个项是一个类型值而不是字节。只要没有歧义，我们可以简单地连接每列的编码字节。

## 9.3 范围查询

`Scanner`是 B+树迭代器的包装器。它将 KVs 解码为行。

```go
// within the range or not?
func (sc *Scanner) Valid() bool
// move the underlying B-tree iterator
func (sc *Scanner) Next()
// fetch the current row
func (sc *Scanner) Deref(rec *Record)

func (db *DB) Scan(table string, req *Scanner) error
```

输入是主键的区间。

```go
type Scanner struct {
    // the range, from Key1 to Key2
    Cmp1 int // CMP_??
    Cmp2 int
    Key1 Record
    Key2 Record
    // ...
}
```

对于开放区间，只需将`Key2`设置为最大/最小值。

## 9.4 我们学到了什么

+   B+树迭代器。

+   顺序保持编码。

下一步是添加二级索引，这些只是额外的表。
