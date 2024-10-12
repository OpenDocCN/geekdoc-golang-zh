<!--yml
category: 未分类
date: 2024-10-13 06:41:07
-->

# Golang Regex: Use a variable inside a Regular Expression

> 来源：[https://golangbyexample.com/variable-regex-golang/](https://golangbyexample.com/variable-regex-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

The **regexp.MustCompile** function is used to compile the given regex string. So the input to the MustCompile function is a string only. And since it is a string we can concatenate any variable with the rest of the pattern.

For example

```
regex := `b+`
sampleRegexp := regexp.MustCompile("a" + regex)
```

So here we are doing concatenation to get the whole pattern

```
"a" + regex
```

Let’s see a running program for the same.

# **Program**

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	regex := `b+`
	sampleRegexp := regexp.MustCompile("a" + regex)

	match := sampleRegexp.FindString("abb")
	fmt.Println(match)

}
```

**Output**

```
abb
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*