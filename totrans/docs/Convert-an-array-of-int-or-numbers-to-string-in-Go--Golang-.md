<!--yml
category: 未分类
date: 2024-10-13 06:52:48
-->

# Convert an array of int or numbers to string in Go (Golang)

> 来源：[https://golangbyexample.com/array-int-string-golang/](https://golangbyexample.com/array-int-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

In this tutorial, we will see how to convert an array of ints or numbers to a string in Go.

Below is the program for the same.

## **Program**

Here is the program for the same.

```
package main

import (
	"fmt"
	"strconv"
)

func main() {
	sample := []int{2, 1, 3}

	output := ""
	for _, v := range sample {
		temp := strconv.Itoa(v)
		output = output + temp
	}
	fmt.Println(output)
}
```

**Output**

```
213
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –

[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

**Note:** Check out our system design tutorial series [System Design Questions](https://techbyexample.com/system-design-questions/)*