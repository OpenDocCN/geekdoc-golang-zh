<!--yml

类别：未分类

日期：2024-10-13 06:41:07

-->

# Golang 正则表达式：在正则表达式中使用变量

> 来源：[`golangbyexample.com/variable-regex-golang/`](https://golangbyexample.com/variable-regex-golang/)

目录

+   概述

+   程序

# **概述**

**regexp.MustCompile** 函数用于编译给定的正则表达式字符串。因此，传递给 MustCompile 函数的仅是一个字符串。由于它是一个字符串，我们可以将任何变量与其余模式连接起来。

例如

```go
regex := `b+`
sampleRegexp := regexp.MustCompile("a" + regex)
```

因此，在这里我们正在进行连接以获取整个模式。

```go
"a" + regex
```

让我们来看一个运行程序。

# **程序**

```go
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

**输出**

```go
abb
```


