<!--yml
category: 未分类
date: 2024-10-13 06:26:53
-->

# Evaluation of defer arguments in Go (Golang)

> 来源：[https://golangbyexample.com/defer-arguments-evaluation-go/](https://golangbyexample.com/defer-arguments-evaluation-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")
*   [Output](#Output "Output")*  *# **Overview**

**defer** arguments are evaluated at the time defer statement is evaluated

Let’s see a program for that

# **Example**

```
package main

import "fmt"

func main() {
	sample := "abc"

	defer fmt.Printf("In defer sample is: %s\n", sample)
	sample = "xyz"
}
```

# **Output**

```
In defer sample is: abc
```

In the above program when the defer statement was evaluated the value of the **sample** variable was **“abc”**. In the defer function, we print the  sample variable. After the defer statement we change the value of the **sample** variable to **“xyz”**.  But the program outputs **“abc”** instead of **“xyz”** because when the defer arguments were evaluated the value of the  sample variable was **“abc”**.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*