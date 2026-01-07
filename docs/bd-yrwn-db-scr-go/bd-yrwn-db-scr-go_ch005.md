# 04\. B+树节点和插入

## 4.1 设计 B+树节点

### 我们将要做什么

第一个大步骤只是 B+树数据结构，其他数据库关注点将在后面的章节中介绍。我们将从下往上做。

1.  设计一个包含所有必要位的节点格式。

1.  以写时复制的方式操作节点（插入和删除键）。

1.  分裂和合并节点。

1.  树的插入和删除。

### 节点格式

所有 B+树节点的大小相同，以便稍后使用空闲列表。虽然我们目前不会处理磁盘数据，但需要一个具体的节点格式，因为它决定了节点的大小（以字节为单位）和*何时分裂节点*。

一个节点包括：

1.  一个固定大小的标头，它包含：

    +   节点的类型（叶节点或内部节点）。

    +   键的数量。

1.  内部节点指向子节点的指针列表。

1.  KVs 对列表。

1.  KVs 的偏移量列表，可用于二分搜索 KVs。

```go
| type | nkeys |  pointers  |   offsets  | key-values | unused |
|  2B  |   2B  | nkeys * 8B | nkeys * 2B |     ...    |        |
```

这是每个 KV 对的格式。长度后跟数据。

```go
| klen | vlen | key | val |
|  2B  |  2B  | ... | ... |
```

### 简化和限制

我们的目标是学习基础知识，而不是创建一个真正的数据库。所以做了一些简化。

同样的格式用于叶节点和内部节点。这浪费了一些空间：叶节点不需要指针，内部节点不需要值。

一个包含*n*个分支的内部节点包含*n*个键，每个键都是从对应子树的最小键复制的。然而，对于*n*个分支，只需要*n* − 1 个键，正如你将在其他 B 树介绍中看到的那样。额外的键使得可视化更容易。

我们将设置节点大小为 4K，这是典型的操作系统页面大小。然而，键和值可以是任意大的，超过单个节点。应该有一种方法来存储大型 KVs，或者使节点大小可变。这个问题是可解决的，但不是根本性的。所以我们将通过限制 KV 大小来跳过它，使它们始终适合节点内部。

```go
const HEADER = 4

const BTREE_PAGE_SIZE = 4096
const BTREE_MAX_KEY_SIZE = 1000
const BTREE_MAX_VAL_SIZE = 3000

func init() {
    node1max := HEADER + 8 + 2 + 4 + BTREE_MAX_KEY_SIZE + BTREE_MAX_VAL_SIZE
    assert(node1max <= BTREE_PAGE_SIZE) // maximum KV
}
```

键大小限制还确保内部节点可以始终容纳 2 个键。

### 内存中的数据类型

在我们的代码中，一个节点只是由这个格式解释的字节数据块。在没有序列化步骤的情况下，从内存到磁盘的数据移动更简单。

```go
type BNode []byte // can be dumped to the disk
```

### 解耦数据结构从 IO

需要对内存和磁盘上的数据结构进行空间分配/释放。我们可以通过回调来抽象化这一点，这是数据结构与其他数据库之间的边界。

```go
type BTree struct {
    // pointer (a nonzero page number)
    root uint64
    // callbacks for managing on-disk pages
    get func(uint64) []byte // dereference a pointer
    new func([]byte) uint64 // allocate a new page
    del func(uint64)        // deallocate a page
}
```

对于磁盘上的 B+树，数据库文件是由页面（节点）组成的数组，这些页面通过页面号（指针）引用。我们将如下实现这些回调：

+   `get`从磁盘读取一个页面。

+   `new`分配并写入一个新的页面（写时复制）。

+   `del`释放一个页面。

我们可以使用假的回调（模拟）来测试内存中的数据结构，而不需要数据库的其他部分。

## 4.2 解码节点格式

由于节点类型只是字节数据块，我们将定义一些辅助函数来访问它。

```go
| type | nkeys |  pointers  |   offsets  | key-values | unused |
|  2B  |   2B  | nkeys * 8B | nkeys * 2B |     ...    |        |

| klen | vlen | key | val |
|  2B  |  2B  | ... | ... |
```

### 标头

```go
const (
    BNODE_NODE = 1 // internal nodes without values
    BNODE_LEAF = 2 // leaf nodes with values
)

func (node BNode) btype() uint16 {
    return binary.LittleEndian.Uint16(node[0:2])
}
func (node BNode) nkeys() uint16 {
    return binary.LittleEndian.Uint16(node[2:4])
}
func (node BNode) setHeader(btype uint16, nkeys uint16) {
    binary.LittleEndian.PutUint16(node[0:2], btype)
    binary.LittleEndian.PutUint16(node[2:4], nkeys)
}
```

### 子指针

```go
// pointers
func (node BNode) getPtr(idx uint16) uint64 {
    assert(idx < node.nkeys())
    pos := HEADER + 8*idx
    return binary.LittleEndian.Uint64(node[pos:])
}
func (node BNode) setPtr(idx uint16, val uint64)
```

### KV 偏移量和对

格式将所有内容紧凑地打包在一起。找到第 n 个 KV 可以通过逐个读取每个 KV 对来实现。为了使其更简单，我们包含了一个偏移量列表，以在*O*(1)时间内定位第 n 个 KV。这也允许在节点内进行二分搜索。

每个偏移量是 KV 对相对于第一个 KV 的**结束**位置。第一个 KV 的起始偏移量是 0，所以我们使用结束偏移量，它是下一个 KV 的起始偏移量。

```go
// offset list
func offsetPos(node BNode, idx uint16) uint16 {
    assert(1 <= idx && idx <= node.nkeys())
    return HEADER + 8*node.nkeys() + 2*(idx-1)
}
func (node BNode) getOffset(idx uint16) uint16 {
    if idx == 0 {
        return 0
    }
    return binary.LittleEndian.Uint16(node[offsetPos(node, idx):])
}
func (node BNode) setOffset(idx uint16, offset uint16)
```

`kvPos`返回第 n 个 KV 对相对于整个节点的位置。

```go
// key-values
func (node BNode) kvPos(idx uint16) uint16 {
    assert(idx <= node.nkeys())
    return HEADER + 8*node.nkeys() + 2*node.nkeys() + node.getOffset(idx)
}
func (node BNode) getKey(idx uint16) []byte {
    assert(idx < node.nkeys())
    pos := node.kvPos(idx)
    klen := binary.LittleEndian.Uint16(node[pos:])
    return node[pos+4:][:klen]
}
func (node BNode) getVal(idx uint16) []byte
```

它还方便地通过偏移量查找返回节点大小（使用空间）。

```go
// node size in bytes
func (node BNode) nbytes() uint16 {
    return node.kvPos(node.nkeys())
}
```

### 节点内的 KV 查找

“seek”操作用于范围查询和点查询。因此，它们在本质上是一样的。

```go
// returns the first kid node whose range intersects the key. (kid[i] <= key)
// TODO: binary search
func nodeLookupLE(node BNode, key []byte) uint16 {
    nkeys := node.nkeys()
    found := uint16(0)
    // the first key is a copy from the parent node,
    // thus it's always less than or equal to the key.
    for i := uint16(1); i < nkeys; i++ {
        cmp := bytes.Compare(node.getKey(i), key)
        if cmp <= 0 {
            found = i
        }
        if cmp >= 0 {
            break
        }
    }
    return found
}
```

该函数被命名为`nodeLookupLE`因为它使用了**小于或等于**运算符。对于点查询，我们应该使用等于运算符，这是我们稍后可以添加的一步。

## 4.2 更新 B+树节点

### 插入到叶节点

让我们考虑将一个键插入到叶节点。第一步是使用`nodeLookupLE`获取插入位置。然后将所有内容复制到一个新的节点，并添加额外的键。这就是写时复制。

```go
// add a new key to a leaf node
func leafInsert(
    new BNode, old BNode, idx uint16,
    key []byte, val []byte,
) {
    new.setHeader(BNODE_LEAF, old.nkeys()+1) // setup the header
    nodeAppendRange(new, old, 0, 0, idx)
    nodeAppendKV(new, idx, 0, key, val)
    nodeAppendRange(new, old, idx+1, idx, old.nkeys()-idx)
}
```

### 节点复制函数

`nodeAppendRange`复制一个 KV 范围，`nodeAppendKV`复制一个 KV 对。这必须按顺序进行，因为这些函数依赖于之前的偏移量。

```go
// copy a KV into the position
func nodeAppendKV(new BNode, idx uint16, ptr uint64, key []byte, val []byte) {
    // ptrs
    new.setPtr(idx, ptr)
    // KVs
    pos := new.kvPos(idx)
    binary.LittleEndian.PutUint16(new[pos+0:], uint16(len(key)))
    binary.LittleEndian.PutUint16(new[pos+2:], uint16(len(val)))
    copy(new[pos+4:], key)
    copy(new[pos+4+uint16(len(key)):], val)
    // the offset of the next key
    new.setOffset(idx+1, new.getOffset(idx)+4+uint16((len(key)+len(val))))
}

// copy multiple KVs into the position from the old node
func nodeAppendRange(
    new BNode, old BNode,
    dstNew uint16, srcOld uint16, n uint16,
)
```

### 更新内部节点

对于内部节点，子节点的链接总是使用写时复制方案进行更新，如果子节点被拆分，则可以成为多个链接。

```go
// replace a link with one or multiple links
func nodeReplaceKidN(
    tree *BTree, new BNode, old BNode, idx uint16,
    kids ...BNode,
) {
    inc := uint16(len(kids))
    new.setHeader(BNODE_NODE, old.nkeys()+inc-1)
    nodeAppendRange(new, old, 0, 0, idx)
    for i, node := range kids {
        nodeAppendKV(new, idx+uint16(i), tree.new(node), node.getKey(0), nil)
        //                ^position      ^pointer        ^key            ^val
    }
    nodeAppendRange(new, old, idx+inc, idx+1, old.nkeys()-(idx+1))
}
```

注意，`tree.new`回调用于分配子节点。

## 4.3 拆分 B+树节点

由于我们施加的大小限制，一个节点可以至少容纳 1 个 KV 对。在最坏的情况下，过大的节点将被拆分为 3 个节点，中间有一个大的 KV。因此，我们可能需要拆分两次。

```go
// split a oversized node into 2 so that the 2nd node always fits on a page
func nodeSplit2(left BNode, right BNode, old BNode) {
    // code omitted...
}

// split a node if it's too big. the results are 1~3 nodes.
func nodeSplit3(old BNode) (uint16, [3]BNode) {
    if old.nbytes() <= BTREE_PAGE_SIZE {
        old = old[:BTREE_PAGE_SIZE]
        return 1, [3]BNode{old} // not split
    }
    left := BNode(make([]byte, 2*BTREE_PAGE_SIZE)) // might be split later
    right := BNode(make([]byte, BTREE_PAGE_SIZE))
    nodeSplit2(left, right, old)
    if left.nbytes() <= BTREE_PAGE_SIZE {
        left = left[:BTREE_PAGE_SIZE]
        return 2, [3]BNode{left, right} // 2 nodes
    }
    leftleft := BNode(make([]byte, BTREE_PAGE_SIZE))
    middle := BNode(make([]byte, BTREE_PAGE_SIZE))
    nodeSplit2(leftleft, middle, left)
    assert(leftleft.nbytes() <= BTREE_PAGE_SIZE)
    return 3, [3]BNode{leftleft, middle, right} // 3 nodes
}
```

注意，返回的节点是从内存中分配的；它们只是直到`nodeReplaceKidN`实际分配它们的临时数据。

## 4.4 B+树插入

我们已经实现了 3 种节点操作：

+   `leafInsert`更新叶节点。

+   `nodeReplaceKidN`更新内部节点。

+   `nodeSplit3`拆分过大的节点。

让我们将它们组合起来进行完整的 B+树插入，这从根节点中的键查找开始，直到达到叶节点。

```go
// insert a KV into a node, the result might be split.
// the caller is responsible for deallocating the input node
// and splitting and allocating result nodes.
func treeInsert(tree *BTree, node BNode, key []byte, val []byte) BNode {
    // the result node.
    // it's allowed to be bigger than 1 page and will be split if so
    new := BNode{data: make([]byte, 2*BTREE_PAGE_SIZE)}

    // where to insert the key?
    idx := nodeLookupLE(node, key)
    // act depending on the node type
    switch node.btype() {
    case BNODE_LEAF:
        // leaf, node.getKey(idx) <= key
        if bytes.Equal(key, node.getKey(idx)) {
            // found the key, update it.
            leafUpdate(new, node, idx, key, val)
        } else {
            // insert it after the position.
            leafInsert(new, node, idx+1, key, val)
        }
    case BNODE_NODE:
        // internal node, insert it to a kid node.
        nodeInsert(tree, new, node, idx, key, val)
    default:
        panic("bad node!")
    }
    return new
}
```

`leafUpdate`与`leafInsert`类似；它更新现有的键而不是插入重复的键。

```go
// part of the treeInsert(): KV insertion to an internal node
func nodeInsert(
    tree *BTree, new BNode, node BNode, idx uint16,
    key []byte, val []byte,
) {
    kptr := node.getPtr(idx)
    // recursive insertion to the kid node
    knode := treeInsert(tree, tree.get(kptr), key, val)
    // split the result
    nsplit, split := nodeSplit3(knode)
    // deallocate the kid node
    tree.del(kptr)
    // update the kid links
    nodeReplaceKidN(tree, new, node, idx, split[:nsplit]...)
}
```

内部节点是递归处理的，每次调用都返回一个更新的节点，如果节点过大，调用者将对其进行拆分并处理分配/释放。

## 4.5 接下来是什么？

工作几乎完成。我们只需在下一章中添加这些内容：

1.  节点合并和树删除。

1.  这是一个高级接口。

1.  测试的假节点回调。
