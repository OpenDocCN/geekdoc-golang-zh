<!--yml

类别：未分类

日期：2024-10-13 06:11:28

-->

# 在Go中实现while循环（Golang）

> 来源：[https://golangbyexample.com/go-implement-while-loop/](https://golangbyexample.com/go-implement-while-loop/)

Go没有**while**关键字。相反，它只有**for**关键字。然而，**for**关键字可以用来模拟与**while**相同的功能。

GO中的**for**循环基本上有三个部分：

```
for initialization_part; condition_part; increment_part {
   ...
}
```

如果**initialization_part**和**increment_part**可以被跳过，**for**循环可以被实现为与**while**行为相同。以下是一个示例：

```
package main

import "fmt"

func main() {
    i := 1
    for i <= 5 {
        fmt.Println(i)
        i++
    }
}
```

**输出：**

```
1
2
3
4
5
```

+   [golang](https://golangbyexample.com/tag/golang/)
