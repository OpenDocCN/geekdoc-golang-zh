<!--yml
category: 未分类
date: 2024-10-13 06:05:18
-->

# Set implementation in Golang

> 来源：[https://golangbyexample.com/set-implementation-in-golang/](https://golangbyexample.com/set-implementation-in-golang/)

A set is a data structure that holds elements without any particular order. An element only appears once in a set.

Set can be implemented in GO using a map. We will be using **map[string]struct{}** for the set as **struct{}** occupies no memory hence more efficient in terms of storage.
Below is the simple example of set having operations

1.  Add
2.  Remove
3.  Exists

```
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

**Output:**

```
Add: B
Add: B
Size: 2
A Exists?: true
B Exists?: true
C Exists?: false
Remove: B
B Exists?: false
```

*   [data structure](https://golangbyexample.com/tag/data-structure/)*   [set](https://golangbyexample.com/tag/set/)