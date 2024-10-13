<!--yml

类别：未分类

日期：2024-10-13 06:05:18

-->

# Golang 中的集合实现

> 来源：[`golangbyexample.com/set-implementation-in-golang/`](https://golangbyexample.com/set-implementation-in-golang/)

集合是一种数据结构，它无特定顺序地存放元素。元素在集合中只出现一次。

集合可以在 GO 中使用地图实现。我们将使用**map[string]struct{}**作为集合，因为**struct{}**占用的内存为零，因此在存储方面更高效。

以下是具有操作的集合的简单示例

1.  添加

1.  删除

1.  存在

```go
package main

import (
    "fmt"
)

//MakeSet initialize the set
func makeSet() *customSet {
    return &customSet{
        container: make(map[string]struct{}),
    }
}

type customSet struct {
    container map[string]struct{}
}

func (c *customSet) Exists(key string) bool {
    _, exists := c.container[key]
    return exists
}

func (c *customSet) Add(key string) {
    c.container[key] = struct{}{}
}

func (c *customSet) Remove(key string) error {
    _, exists := c.container[key]
    if !exists {
        return fmt.Errorf("Remove Error: Item doesn't exist in set")
    }
    delete(c.container, key)
    return nil
}

func (c *customSet) Size() int {
    return len(c.container)
}

func main() {
    customSet := makeSet()
    fmt.Printf("Add: B\n")
    customSet.Add("A")
    fmt.Printf("Add: B\n")
    customSet.Add("B")
    fmt.Printf("Size: %d\n", customSet.Size())
    fmt.Printf("A Exists?: %t\n", customSet.Exists("A"))
    fmt.Printf("B Exists?: %t\n", customSet.Exists("B"))
    fmt.Printf("C Exists?: %t\n", customSet.Exists("C"))
    fmt.Printf("Remove: B\n")
    customSet.Remove("B")
    fmt.Printf("B Exists?: %t\n", customSet.Exists("B"))
}
```

**输出：**

```go
Add: B
Add: B
Size: 2
A Exists?: true
B Exists?: true
C Exists?: false
Remove: B
B Exists?: false
```

+   [数据结构](https://golangbyexample.com/tag/data-structure/) *   [集合](https://golangbyexample.com/tag/set/)
