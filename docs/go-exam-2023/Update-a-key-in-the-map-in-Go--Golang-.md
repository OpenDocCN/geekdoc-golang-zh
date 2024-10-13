<!--yml

类别：未分类

日期：2024-10-13 06:20:51

-->

# 更新 Go (Golang) 中映射中的键

> 来源：[https://golangbyexample.com/update-key-map-golang/](https://golangbyexample.com/update-key-map-golang/)

当尝试向映射中添加一个已经存在的键时，新值会覆盖旧值。这类似于在映射中更新键。

让我们看看一个例子

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)

    //Adding a key value
    fmt.Println("Before update")
    employeeSalary["Tom"] = 2000
    fmt.Println(employeeSalary)

    fmt.Println("After update")
    employeeSalary["Tom"] = 3000
    fmt.Println(employeeSalary)
}
```

**输出**

```
Before update
map[Tom:2000]
After update
map[Tom:3000]
```

在上面的程序中，当用新值**“3000”**写入相同的键时，它会覆盖现有值**“2000”**。当我们再次打印映射时，打印的值为3000。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)
