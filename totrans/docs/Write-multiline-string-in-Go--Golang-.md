<!--yml
category: 未分类
date: 2024-10-13 06:10:09
-->

# Write multiline string in Go (Golang)

> 来源：[https://golangbyexample.com/multiline-string-go/](https://golangbyexample.com/multiline-string-go/)

A backquote (**`**) can be used to write a multiline string in Golang. Note that a string encoded in backquotes is a raw literal string and doesn’t honor any kind of escaping. Thus \n, \t are treated as a string literal when using back quotes

**Working Code:**

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

**Output**

```
This is 
a multiline 
string
```

*   [golang](https://golangbyexample.com/tag/golang/)