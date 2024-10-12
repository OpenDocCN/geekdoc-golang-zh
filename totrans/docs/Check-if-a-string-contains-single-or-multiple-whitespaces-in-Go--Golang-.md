<!--yml
category: 未分类
date: 2024-10-13 06:52:44
-->

# Check if a string contains single or multiple whitespaces in Go (Golang)

> 来源：[https://golangbyexample.com/string-whitespace-golang/](https://golangbyexample.com/string-whitespace-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

A simple regex can be used to check if a string contains single or multiple white spaces in Go.

Here is the program for the same

# **Program**

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	//Single whitespace
	sampleWord := "Good morning"

	isWhitespacePresent := regexp.MustCompile(`\s`).MatchString(sampleWord)
	fmt.Println(isWhitespacePresent)

	//Multiple Whitespace
	sampleWord = "Good   morning"
	isMultipleWhitespacePresent := regexp.MustCompile(`\s*`).MatchString(sampleWord)
	fmt.Println(isMultipleWhitespacePresent)
}
```

**Output**

```
true
true
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –

[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

**Note:** Check out our system design tutorial series [System Design Questions](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*