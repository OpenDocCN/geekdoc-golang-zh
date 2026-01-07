# 05\. B+树删除和测试

## 5.1 高级接口

我们将添加使用 B+树作为 KV 的接口。

```go
// insert a new key or update an existing key
func (tree *BTree) Insert(key []byte, val []byte)
// delete a key and returns whether the key was there
func (tree *BTree) Delete(key []byte) bool
```

大多数细节都是在树插入时介绍的，所以从删除中学习的内容并不多。如果你已经知道原理，可以跳过这一章。

### 保留根节点

在树插入时维护根节点有一些额外的工作。

+   如果树为空，则创建根节点。

+   如果根节点被分割，添加一个新的根节点。

```go
func (tree *BTree) Insert(key []byte, val []byte) {
    if tree.root == 0 {
        // create the first node
        root := BNode(make([]byte, BTREE_PAGE_SIZE))
        root.setHeader(BNODE_LEAF, 2)
        // a dummy key, this makes the tree cover the whole key space.
        // thus a lookup can always find a containing node.
        nodeAppendKV(root, 0, 0, nil, nil)
        nodeAppendKV(root, 1, 0, key, val)
        tree.root = tree.new(root)
        return
    }

    node := treeInsert(tree, tree.get(tree.root), key, val)
    nsplit, split := nodeSplit3(node)
    tree.del(tree.root)
    if nsplit > 1 {
        // the root was split, add a new level.
        root := BNode(make([]byte, BTREE_PAGE_SIZE))
        root.setHeader(BNODE_NODE, nsplit)
        for i, knode := range split[:nsplit] {
            ptr, key := tree.new(knode), knode.getKey(0)
            nodeAppendKV(root, uint16(i), ptr, key, nil)
        }
        tree.root = tree.new(root)
    } else {
        tree.root = tree.new(split[0])
    }
}
```

### 哨兵值

在创建第一个根节点时有一个技巧：我们插入了一个空键。这被称为哨兵值，它用于消除一个边缘情况。

如果你检查查找函数`nodeLookupLE`，你会看到如果键不在节点范围内，它将不起作用。这是通过在树中插入一个空键来修复的，这是按排序顺序最低的可能键，这样`nodeLookupLE`总能找到一个位置。

## 5.2 合并节点

### 节点更新函数

我们将需要一些新的函数来进行树删除。

```go
// remove a key from a leaf node
func leafDelete(new BNode, old BNode, idx uint16)
// merge 2 nodes into 1
func nodeMerge(new BNode, left BNode, right BNode)
// replace 2 adjacent links with 1
func nodeReplace2Kid(
    new BNode, old BNode, idx uint16, ptr uint64, key []byte,
)
```

### 合并条件

删除可能会导致空节点，如果它有一个兄弟节点，则可以与其合并。`shouldMerge`函数返回要合并的兄弟节点（左或右）。

```go
// should the updated kid be merged with a sibling?
func shouldMerge(
    tree *BTree, node BNode,
    idx uint16, updated BNode,
) (int, BNode) {
    if updated.nbytes() > BTREE_PAGE_SIZE/4 {
        return 0, BNode{}
    }

    if idx > 0 {
        sibling := BNode(tree.get(node.getPtr(idx - 1)))
        merged := sibling.nbytes() + updated.nbytes() - HEADER
        if merged <= BTREE_PAGE_SIZE {
            return -1, sibling // left
        }
    }
    if idx+1 < node.nkeys() {
        sibling := BNode(tree.get(node.getPtr(idx + 1)))
        merged := sibling.nbytes() + updated.nbytes() - HEADER
        if merged <= BTREE_PAGE_SIZE {
            return +1, sibling // right
        }
    }
    return 0, BNode{}
}
```

被删除的键意味着节点内的未使用空间。在最坏的情况下，一个大部分为空的树仍然可以保留大量的节点。我们可以通过提前触发合并来改进这一点——使用 1/4 页作为阈值而不是空节点，这是对最小节点大小的软限制。

## 5.3 B+树删除

这与插入类似，只是用合并替换分割。

```go
// delete a key from the tree
func treeDelete(tree *BTree, node BNode, key []byte) BNode

// delete a key from an internal node; part of the treeDelete()
func nodeDelete(tree *BTree, node BNode, idx uint16, key []byte) BNode {
    // recurse into the kid
    kptr := node.getPtr(idx)
    updated := treeDelete(tree, tree.get(kptr), key)
    if len(updated) == 0 {
        return BNode{} // not found
    }
    tree.del(kptr)

    new := BNode(make([]byte, BTREE_PAGE_SIZE))
    // check for merging
    mergeDir, sibling := shouldMerge(tree, node, idx, updated)
    switch {
    case mergeDir < 0: // left
        merged := BNode(make([]byte, BTREE_PAGE_SIZE))
        nodeMerge(merged, sibling, updated)
        tree.del(node.getPtr(idx - 1))
        nodeReplace2Kid(new, node, idx-1, tree.new(merged), merged.getKey(0))
    case mergeDir > 0: // right
        merged := BNode(make([]byte, BTREE_PAGE_SIZE))
        nodeMerge(merged, updated, sibling)
        tree.del(node.getPtr(idx + 1))
        nodeReplace2Kid(new, node, idx, tree.new(merged), merged.getKey(0))
    case mergeDir == 0 && updated.nkeys() == 0:
        assert(node.nkeys() == 1 && idx == 0) // 1 empty child but no sibling
        new.setHeader(BNODE_NODE, 0)          // the parent becomes empty too
    case mergeDir == 0 && updated.nkeys() > 0: // no merge
        nodeReplaceKidN(tree, new, node, idx, updated)
    }
    return new
}
```

即使一个节点变为空，如果没有兄弟节点，它可能不会被合并。在这种情况下，空节点将被传播到其父节点，并在稍后合并。

## 5.4 测试 B+树

数据结构只通过 3 个页面管理回调与数据库的其余部分交互。为了测试 B+树，我们可以在内存中模拟页面。

```go
type C struct {
    tree  BTree
    ref   map[string]string // the reference data
    pages map[uint64]BNode  // in-memory pages
}

func newC() *C {
    pages := map[uint64]BNode{}
    return &C{
        tree: BTree{
            get: func(ptr uint64) []byte {
                node, ok := pages[ptr]
                assert(ok)
                return node
            },
            new: func(node []byte) uint64 {
                assert(BNode(node).nbytes() <= BTREE_PAGE_SIZE)
                ptr := uint64(uintptr(unsafe.Pointer(&node[0])))
                assert(pages[ptr] == nil)
                pages[ptr] = node
                return ptr
            },
            del: func(ptr uint64) {
                assert(pages[ptr] != nil)
                delete(pages, ptr)
            },
        },
        ref:   map[string]string{},
        pages: pages,
    }
}
```

`C.pages`是一个已分配页面的映射。它用于验证指针和读取页面。指针实际上是内存指针，B+树代码并不关心。

要测试 B+树，我们首先需要在各种场景下更新它，然后验证结果。验证是通用的，有两件事需要验证：

1.  结构是有效的。

    +   键是有序的。

    +   节点大小在限制范围内。

1.  数据与参考匹配。我们使用映射来捕获每个更新。

```go
func (c *C) add(key string, val string) {
    c.tree.Insert([]byte(key), []byte(val))
    c.ref[key] = val // reference data
}
```

测试案例留作练习。接下来是磁盘上的 B+树。
