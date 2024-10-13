<!--yml

分类：未分类

日期：2024-10-13 06:52:20

-->

# 在 Go (Golang)中以升序和降序排序整型切片

> 来源：[`golangbyexample.com/sort-slice-asc-desc-golang/`](https://golangbyexample.com/sort-slice-asc-desc-golang/)

目录

**   以升序排序切片

+   以降序排序切片*  *# **以升序排序切片**

**sort.Ints**包可以用来排序整个切片或切片的一部分。它将字符串排序为升序。

下面是该方法的签名。

[`pkg.go.dev/sort#Ints`](https://pkg.go.dev/sort#Ints)

```go
func Ints(x []int)
```

它将切片‘x’作为参数，并对切片‘x’进行就地排序。

下面是相同程序的代码。

```go
package main

import (
	"fmt"
	"sort"
)

func main() {

	nums := []int{3, 4, 2, 1}
	fmt.Printf("Before: %v", nums)
	sort.Ints(nums)
	fmt.Printf("\nAfter: %v", nums)
}
```

**输出**

```go
Before: [3 4 2 1]
After: [1 2 3 4]
```

# **以降序排序切片**

为了以降序对切片进行排序，我们将使用**sort**包的**Slice**方法。

[`pkg.go.dev/sort#Slice`](https://pkg.go.dev/sort#Slice)

下面是该方法的签名。

```go
func Slice(x any, less func(i, j int) bool)
```

这个函数接受两个参数

+   **x any** – 这里的 any 只是一个空接口 [`pkg.go.dev/builtin#any`](https://pkg.go.dev/builtin#any)

+   **less func(i, j int)** – 这个函数实际上就是一个比较函数。

我们可以使用这个比较函数来决定切片中元素的降序。下面是一个例子。

```go
package main

import (
	"fmt"
	"sort"
)

func main() {

	nums := []int{3, 4, 2, 1}
	fmt.Printf("Before: %v", nums)
	sort.Slice(nums, func(i, j int) bool {
		return nums[i] > nums[j]
	})
	fmt.Printf("\nAfter: %v", nums)
}
```

**输出**

```go
Before: [3 4 2 1]
After: [4 3 2 1]
```

实际上，你也可以使用**sort.Slice**方法以降序排序切片。下面是一个例子。

```go
package main

import (
	"fmt"
	"sort"
)

func main() {

	nums := []int{3, 4, 2, 1}
	fmt.Printf("Before: %v", nums)
	sort.Slice(nums, func(i, j int) bool {
		return nums[i] < nums[j]
	})
	fmt.Printf("\nAfter: %v", nums)
}
```

**输出**

```go
Before: [3 4 2 1]
After: [1 2 3 4]
```

**注意：** 查看我们的 Golang 高级教程。该系列教程内容详尽，我们尽力覆盖所有概念并提供示例。这个教程适合那些希望获得专业知识和扎实理解 Golang 的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，这篇文章就是为你准备的 -

[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

**注意：** 查看我们的系统设计教程系列 [系统设计问题](https://techbyexample.com/system-design-questions/)

+   [go](https://golangbyexample.com/tag/go/)*
