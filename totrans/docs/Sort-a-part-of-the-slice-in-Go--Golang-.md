<!--yml
category: 未分类
date: 2024-10-13 06:43:04
-->

# Sort a part of the slice in Go (Golang)

> 来源：[https://golangbyexample.com/sort-part-slice-go/](https://golangbyexample.com/sort-part-slice-go/)

**Overview**

**sort.Slice** package of golang can be used to sort a full slice or a part of the sliceFull slice sorting in asc order

```
package main

import (
	"fmt"
	"sort"
)

func main() {

	nums := []int{3, 4, 2, 1}
	sort.Slice(nums, func(i, j int) bool {
		return nums[i] < nums[j]
	})
	fmt.Println(nums)
}
```

**Output**

```
[1,2,3,4]
```

Full slice sorting in desc order

```
package main
import (
    "fmt"
    "sort"
)
func main() {
    nums := []int{3, 4, 2, 1}
    sort.Slice(nums, func(i, j int) bool {
        return nums[i] > nums[j]
    })
    fmt.Println(nums)
}
```

**Output**

```
[4 3 2 1]
```

Part slice sorting

```
package main

import (
	"fmt"
	"sort"
)

func main() {

	nums := []int{3, 4, 2, 1}
	start := 2
	end := 4
	sort.Slice(nums[start:end], func(i, j int) bool {
		return nums[start+i] < nums[start+j]
	})
	fmt.Println(nums)
}
```

**Output**

```
[3 4 1 2]
```

Notice that it has sorted the last two elements in the asc order.

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)