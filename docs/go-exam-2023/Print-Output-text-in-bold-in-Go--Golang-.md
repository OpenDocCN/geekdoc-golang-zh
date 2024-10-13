<!--yml

category: 未分类

date: 2024-10-13 06:41:13

-->

# 在Go（Golang）中以粗体打印/输出文本

> 来源：[https://golangbyexample.com/print-text-bold-go/](https://golangbyexample.com/print-text-bold-go/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

我们可以使用faith包来实现相同的功能

[https://github.com/fatih/color](https://github.com/fatih/color)

# **程序**

```
package main

import "github.com/fatih/color"

func main() {
    whilte := color.New(color.FgWhite)
    boldWhite := whilte.Add(color.Bold)
    boldWhite.Println("This will print text in bold white.")
}
```

**输出**

![](img/a60bfb1e239c23e59b81528874806bee.png)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
