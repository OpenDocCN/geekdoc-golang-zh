<!--yml
category: 未分类
date: 2024-10-13 06:21:02
-->

# Format specifier for boolean or print boolean in Go (Golang)

> 来源：[https://golangbyexample.com/format-specifier-for-bool-golang/](https://golangbyexample.com/format-specifier-for-bool-golang/)

Different format specifiers can be used to print a boolean in either bool or string.

*   **%t** can be used to print in the boolean form
*   **%v** will print the default string. “true” for true and “false” for false

**Code:**

```
package main

import "fmt"

func main() {
	t := true
	f := false

	fmt.Printf("%t %t\n", t, f)
	fmt.Printf("%v %v\n", t, f)
}
```

**Output**

```
true false
true false
```

*   [bool](https://golangbyexample.com/tag/bool/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)