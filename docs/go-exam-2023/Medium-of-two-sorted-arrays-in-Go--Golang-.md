<!--yml

类别：未分类

日期：2024-10-13 06:42:00

-->

# 在Go（Golang）中两个已排序数组的中位数

> 来源：[https://golangbyexample.com/medium-two-sorted-arrays-golang/](https://golangbyexample.com/medium-two-sorted-arrays-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

目标是返回两个已排序数组的中位数。例如，如果输入是

```go
[1,5]
[3,4,6]
```

那么中位数是4，因为如果我们对两个数组进行排序，4正好在中间。

```go
[1,3,4,5,6]
```

如果总元素个数是偶数，也就是说两个数组的总长度是偶数。在这种情况下，中位数将会

```go
(n/2 + (n/2 +1 ))/2
```

其中**n**是两个数组的总长度。

例如，如果输入是

```go
[1,2]
[3,4]
```

那么中位数是**(2+3)/2=2.5**，因为如果我们对两个数组进行排序，2和3正好在中间。

```go
[1,2,3,4]
```

所以如果两个数组的总长度为**n**，那么有两种情况。

+   如果**n**是奇数，那么中位数位于位置**n/2+1**。

+   如果**n**是偶数，那么中位数位于位置**(n/2 + (n/2 +1 ))/2**。

## **程序**

```go
package main

import "fmt"

func main() {
	median := findMedianSortedArrays([]int{1, 2}, []int{3, 4})
	fmt.Println(median)
}

func findMedianSortedArrays(nums1 []int, nums2 []int) float64 {
	firstArrayLen := len(nums1)
	secondArrayLen := len(nums2)

	var mid int
	i := 0
	j := 0
	var k int
	mid = (firstArrayLen+secondArrayLen)/2 + 1

	//This is the case in which the total lenght of two arrays is odd and there is only one median
	if (firstArrayLen+secondArrayLen)%2 == 1 {
		var median float64

		for k < mid {
			if i < firstArrayLen && j < secondArrayLen {
				if nums1[i] <= nums2[j] {
					median = float64(nums1[i])
					i++
					k++
				} else {
					median = float64(nums2[j])
					j++
					k++
				}
			} else if i < firstArrayLen {
				median = float64(nums1[i])
				i++
				k++
			} else {
				median = float64(nums2[j])
				j++
				k++
			}

		}
		return median
	} else { //This is the case in which the total lenght of two arrays is even and there is only two medians. We need to return average of these two medians
		var median1 float64
		var median2 float64

		for k < mid {
			median1 = median2
			if i < firstArrayLen && j < secondArrayLen {
				if nums1[i] <= nums2[j] {
					median2 = float64(nums1[i])
					i++
					k++
				} else {
					median2 = float64(nums2[j])
					j++
					k++
				}
			} else if i < firstArrayLen {
				median2 = float64(nums1[i])
				i++
				k++
			} else {
				median2 = float64(nums2[j])
				j++
				k++
			}

		}
		return (median1 + median2) / 2
	}
}
```

**输出**

```go
2.5
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
