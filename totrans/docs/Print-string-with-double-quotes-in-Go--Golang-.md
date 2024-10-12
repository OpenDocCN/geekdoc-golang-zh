<!--yml
category: 未分类
date: 2024-10-13 06:35:13
-->

# Print string with double quotes in Go (Golang)

> 来源：[https://golangbyexample.com/print-double-quotes-string-golang/](https://golangbyexample.com/print-double-quotes-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program for double-quoted string](#Program_for_double-quoted_string "Program for double-quoted string")*  *## **Overview**

The backslash is the escaping character. To print any string that contains literal double quotes, we need to escape both these quotes when the string is enclosed within double quotes. However, a string enclosed in backquotes is a raw literal string and doesn’t honor any kind of escaping. So back quotes can also be used to print a string with literal double quotes

## **Program for double-quoted string**

```
package main

import "fmt"

func main() {
	fmt.Println("\"test\"")
}
```

**Output**

```
"test"
```

Noticed that we escaped both single quotes and double quotes within that string 

**Program for back quotes**

```
package main
import "fmt"
func main() {
    fmt.Println(`"test"`)
}
```

**Output**

```
"test"
```

*   [go](https://golangbyexample.com/tag/go/)*