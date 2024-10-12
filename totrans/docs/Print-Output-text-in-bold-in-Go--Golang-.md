<!--yml
category: 未分类
date: 2024-10-13 06:41:13
-->

# Print/Output text in bold in Go (Golang)

> 来源：[https://golangbyexample.com/print-text-bold-go/](https://golangbyexample.com/print-text-bold-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

We can use the faith package to achieve the same

[https://github.com/fatih/color](https://github.com/fatih/color)

# **Program**

```
package main

import "github.com/fatih/color"

func main() {
    whilte := color.New(color.FgWhite)
    boldWhite := whilte.Add(color.Bold)
    boldWhite.Println("This will print text in bold white.")
}
```

**Output**

![](img/a60bfb1e239c23e59b81528874806bee.png)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*