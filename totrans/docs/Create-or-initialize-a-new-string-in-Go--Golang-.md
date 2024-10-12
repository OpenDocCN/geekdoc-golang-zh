<!--yml
category: 未分类
date: 2024-10-13 06:52:39
-->

# Create or initialize a new string in Go (Golang)

> 来源：[https://golangbyexample.com/create-string-golang/](https://golangbyexample.com/create-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

Below is a simple way to initialize or create a string in Go. In the below program, we have simply declared as well as defined a string named **sample**

Notice the syntax

```
sample := "test"
```

Here is the full program

# Program

```
package main

import "fmt"

func main() {
	sample := "test"
	fmt.Println(sample)
}
```

**Output**

```
test
```

If we want to only declare a string variable then below is the way. A declared string is initialized to an empty string

```
package main

import "fmt"

func main() {
    var sample string
    fmt.Println(sample)
}
```

**Output**

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –

[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

**Note:** Check out our system design tutorial series [System Design Questions](https://techbyexample.com/system-design-questions/)*