<!--yml

category: 未分类

date: 2024-10-13 06:21:02

-->

# 用于布尔值的格式说明符或在Go（Golang）中打印布尔值。

> 来源：[https://golangbyexample.com/format-specifier-for-bool-golang/](https://golangbyexample.com/format-specifier-for-bool-golang/)

可以使用不同的格式说明符以布尔或字符串形式打印布尔值。

+   **%t** 可用于以布尔形式打印。

+   **%v** 将打印默认字符串。“true”代表真，“false”代表假。

**代码：**

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

**输出**

```
true false
true false
```

+   [布尔值](https://golangbyexample.com/tag/bool/)*   [Go语言](https://golangbyexample.com/tag/go/)*   [Golang](https://golangbyexample.com/tag/golang/)
