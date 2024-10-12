<!--yml
category: 未分类
date: 2024-10-13 06:42:00
-->

# Medium of two sorted arrays in Go (Golang)

> 来源：[https://golangbyexample.com/medium-two-sorted-arrays-golang/](https://golangbyexample.com/medium-two-sorted-arrays-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to return the medium of two sorted arrays. For example, if the input is

```
[1,5]
[3,4,6]
```

Then the median is 4 because if we sort both arrays then 4 is in the middle

```
[1,3,4,5,6]
```

What if there are an even number of total elements. That is if the total length of two arrays is even. In this case, the median would

```
(n/2 + (n/2 +1 ))/2
```

where **n** is the total length of two arrays

For example, if the input is

```
[1,2]
[3,4]
```

Then the median is **(2+3)/2=2.5** because if we sort both arrays then 2 and 3 are in the middle

```
[1,2,3,4]
```

So if the total length of two arrays is **n**, then there are two cases

*   If **n** is odd, then median is at the position **n/2+1**

*   If **n** is even, then median is at the position **(n/2 + (n/2 +1 ))/2**

## **Program**

```
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

**Output**

```
2.5
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*