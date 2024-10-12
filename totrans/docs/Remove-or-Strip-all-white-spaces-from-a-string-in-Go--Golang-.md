<!--yml
category: 未分类
date: 2024-10-13 06:10:04
-->

# Remove or Strip all white spaces from a string in Go (Golang)

> 来源：[https://golangbyexample.com/remove-all-white-spaces-string-golang/](https://golangbyexample.com/remove-all-white-spaces-string-golang/)

**strings.ReplaceAll** function can be used to trim all white spaces from a string in Golang. Below is the signature of the function

```
func ReplaceAll(s, old, new string) string
```

*   **s**(first argument) is the input string
*   **old**(second argument) is the string that has to be replaced with **new**(third argument) string

**Working Code:**

```
package main

import (
    "fmt"
    "strings"
)

func main() {
    sample := " This is a sample string   "
    noSpaceString := strings.ReplaceAll(sample, " ", "")
    fmt.Println(noSpaceString)
}
```

**Output:**

```
Thisisasamplestring
```

*   [all](https://golangbyexample.com/tag/all/)*   [trim](https://golangbyexample.com/tag/trim/)