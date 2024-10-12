<!--yml
category: 未分类
date: 2024-10-13 06:50:42
-->

# Detonate the maximum bombs program in Go (Golang)

> 来源：[https://golangbyexample.com/detonate-maximum-bombs-golang/](https://golangbyexample.com/detonate-maximum-bombs-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

A 2d array  is given where each entry in an  array has three values

*   i- Denotes the x coordinates of the bomb

*   j – Denotes the y  coordinates of the  bomb

*   r –  Denotes the radius of the range of the bomb

A bomb when exploded would cause an explosion of all the bombs within its range.  When those bombs are exploded they will in turn cause all the bombs in their range to be exploded.

You can only detonate one bomb. Idea is to find the maximum number of bombs  that  can be detonated

**Example 1**

```
Input: [[1,0,3],[5,0,4]]
Output: 2
```

The first bomb lies in the range of the second bomb. So when we explode the  second bomb, both the second  and first bomb will  be exploded

**Example 2**

```
Input:  [[2,2,2],[10,10,5]]
Output: 1
```

Both the bomb are outside the range of each other. 

The idea to solve this problem is to view everything as a directed graph where a directed node from the first bomb to the second bomb exists if the second bomb lies within the range of the first bomb.

Once this graph is constructed, we can do DFS  from each node to get the maximum number of bombs it can explode. We will also store the results previously computed as well.

# **Program**

Below is the program for the same

```
package main

import (
	"fmt"
	"math"
)

func maximumDetonation(bombs [][]int) int {

	if len(bombs) == 0 {
		return 0
	}
	max := 1
	detonationMap := make(map[int][]int)

	for i := 0; i < len(bombs); i++ {
		for j := 0; j < len(bombs); j++ {
			if i != j {
				if float64(bombs[i][2]) >= distance(bombs[i], bombs[j]) {
					if arr, ok := detonationMap[i]; ok {
						arr = append(arr, j)
						detonationMap[i] = arr
					} else {
						var arr []int
						arr = append(arr, j)
						detonationMap[i] = arr
					}
				}
			}
		}
	}

	for key := range detonationMap {
		detonated := 1
		queue := []int{key}
		visited := make(map[int]bool)
		visited[key] = true

		for len(queue) > 0 {
			cur := queue[0]
			queue = queue[1:]

			for _, val := range detonationMap[cur] {
				if !visited[val] {
					detonated++
					visited[val] = true
					queue = append(queue, val)
				}
			}
		}
		if detonated == len(bombs) {
			return len(bombs)
		}

		if detonated > max {
			max = detonated
		}
	}
	return max
}

func distance(a []int, b []int) float64 {
	ret := math.Sqrt(math.Pow(float64(a[0]-b[0]), 2) + math.Pow(float64(a[1]-b[1]), 2))
	return ret
}

func main() {
	output := maximumDetonation([][]int{{1, 0, 3}, {5, 0, 4}})
	fmt.Println(output)

	output = maximumDetonation([][]int{{2, 2, 2}, {10, 10, 5}})
	fmt.Println(output)

}
```

**Output:**

```
2
1
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you – [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here – [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*