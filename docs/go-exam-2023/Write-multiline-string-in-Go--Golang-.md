<!--yml

类别: 未分类

日期: 2024-10-13 06:10:09

-->

# 在Go (Golang)中编写多行字符串

> 来源：[https://golangbyexample.com/multiline-string-go/](https://golangbyexample.com/multiline-string-go/)

反引号(**`**)可以用于在Golang中编写多行字符串。请注意，用反引号编码的字符串是原始字面量字符串，不会遵循任何类型的转义。因此，在使用反引号时，\n和\t被视为字符串字面量。

**工作代码:**

```
package main

import "fmt"

func main() {
    multiline := `This is 
a multiline 
string`

    fmt.Println(multiline)
}
```

**输出**

```
This is 
a multiline 
string
```

+   [golang](https://golangbyexample.com/tag/golang/)
