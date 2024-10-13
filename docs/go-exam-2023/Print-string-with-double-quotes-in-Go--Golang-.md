<!--yml

分类：未分类

日期：2024-10-13 06:35:13

-->

# 在 Go (Golang) 中用双引号打印字符串

> 来源：[`golangbyexample.com/print-double-quotes-string-golang/`](https://golangbyexample.com/print-double-quotes-string-golang/)

目录

+   概述**

+   双引号字符串的程序

## **概述**

反斜杠是转义字符。要打印包含字面双引号的任何字符串，我们需要在字符串用双引号括起来时转义这两个引号。然而，用反引号括起来的字符串是原始字面字符串，不会遵循任何转义。因此，反引号也可以用来打印包含字面双引号的字符串。

## **双引号字符串的程序**

```go
package main

import "fmt"

func main() {
	fmt.Println("\"test\"")
}
```

**输出**

```go
"test"
```

注意到我们在那个字符串中转义了单引号和双引号

**反引号的程序**

```go
package main
import "fmt"
func main() {
    fmt.Println(`"test"`)
}
```

**输出**

```go
"test"
```


