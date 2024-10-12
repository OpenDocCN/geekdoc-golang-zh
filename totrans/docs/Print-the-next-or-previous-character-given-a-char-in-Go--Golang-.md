<!--yml
category: 未分类
date: 2024-10-13 06:52:10
-->

# Print the next or previous character given a char in Go (Golang)

> 来源：[https://golangbyexample.com/next-previous-char-golang/](https://golangbyexample.com/next-previous-char-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

Simply by doing plus 1 and minus 1 we can get the next and previous character given a current char.

# **Program**

Here is the program for the same

```
package main

import "fmt"

func main() {
	var curr byte
	curr = 'b'

	fmt.Println(string(curr + 1))
	fmt.Println(string(curr - 1))
} 
```

**Output:**

```
c
a
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you – [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*