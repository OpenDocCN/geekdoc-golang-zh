<!--yml

类别：未分类

日期：2024-10-13 06:43:04

-->

# 在Go (Golang)中排序部分切片

> 来源：[https://golangbyexample.com/sort-part-slice-go/](https://golangbyexample.com/sort-part-slice-go/)

**概述**

Golang的**sort.Slice**包可用于排序完整切片或部分切片，完整切片按升序排序

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

**输出**

```
[1,2,3,4]
```

完整切片按降序排序

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

**输出**

```
[4 3 2 1]
```

部分切片排序

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

**输出**

```
[3 4 1 2]
```

请注意，它已将最后两个元素按升序排列。

**注意：** 请查看我们的Golang高级教程。本系列教程内容详尽，力求用实例覆盖所有概念。该教程适合希望获得专业知识和对Golang有扎实理解的学习者 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)。

如果你有兴趣了解如何在Golang中实现所有设计模式。那么这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)。

+   [go](https://golangbyexample.com/tag/go/) * [golang](https://golangbyexample.com/tag/golang/)
