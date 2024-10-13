<!--yml

分类：未分类

日期：2024-10-13 06:44:38

-->

# 在 Go 中对 0、1 和 2 的数组进行排序。

> 来源：[`golangbyexample.com/sort-array-one-zero-two-golang/`](https://golangbyexample.com/sort-array-one-zero-two-golang/)

目录

**   概述

+   第一种方法 - 使用计数

+   第二种方法 - 使用交换和洗牌*  *## **概述**

目标是对一个包含 0、1 和 2 的数组进行排序，使所有的 0 在开头，所有的 1 在中间，所有的 2 在最后。该解决方案的空间复杂度应为 O(1)，这意味着不应在给定数组的输入大小上分配任何额外空间。

例如

```go
Input: [0, 1, 2, 0, 1]
Output: [0, 0, 1, 1, 2]

Input: [1, 0]
Output: [0, 1]
```

解决此问题有两种方法。

**第一种方法：**我们可以遍历数组并记录 0、1 和 2 的数量。一旦我们有了计数，再次遍历数组并将该数量的 0 先放入，然后是 1 的数量，最后是 2 的数量。

**第二种方法：**在第二种方法中，我们可以使用三个索引。

+   low 初始化为 0。

+   mid 初始化为 0。

+   high 初始化为输入数组长度减一。

现在遍历数组的 mid 位置。

+   如果遇到 0，我们将其移动到位置**low**的左侧。然后递增**low**和**mid**。

+   如果遇到 1，我们简单地执行**mid++**。

+   如果遇到 2，我们将其简单地移动到位置**high**的右侧。然后递减**high**。

这里是相同的程序。

## **第一种方法 - 使用计数**

```go
package main

import "fmt"

func main() {
	sortNums([]int{2, 0, 2, 1, 1, 0})
}

func sortNums(nums []int) {
	zeroCount := 0
	oneCount := 0
	twoCount := 0

	for i := 0; i < len(nums); i++ {
		switch nums[i] {
		case 0:
			zeroCount++
		case 1:
			oneCount++
		case 2:
			twoCount++
		}
	}
	counter := 0
	for i := 0; i < zeroCount; i++ {
		nums[counter] = 0
		counter++
	}
	for i := 0; i < oneCount; i++ {
		nums[counter] = 1
		counter++
	}
	for i := 0; i < twoCount; i++ {
		nums[counter] = 2
		counter++
	}
	fmt.Println(nums)
}
```

**输出**

```go
[0 0 1 1 2 2]
```

## **第二种方法 - 使用交换和洗牌**

```go
package main

import "fmt"

func main() {
	sortNums([]int{2, 0, 2, 1, 1, 0})
}

func sortNums(nums []int) {
	low := 0
	mid := 0
	high := len(nums) - 1

	for mid <= high {

		switch nums[mid] {
		case 0:
			nums[low], nums[mid] = nums[mid], nums[low]
			low++
			mid++
		case 1:
			mid++
		case 2:
			nums[mid], nums[high] = nums[high], nums[mid]
			high--
		}
	}
	fmt.Println(nums)
}
```

**输出**

```go
[0 0 1 1 2 2]
```

**注意：**查看我们的 Golang 高级教程。本系列的教程内容详尽，我们尝试涵盖所有概念并配有示例。该教程适合那些希望获得专业知识并深入理解 Golang 的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是，那么这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
