<!--yml

类别：未分类

日期：2024-10-13 06:18:17

-->

# Go 中的 For 循环（Golang）

> 来源：[https://golangbyexample.com/for-loop-in-golang/](https://golangbyexample.com/for-loop-in-golang/)

这是 Golang 综合教程系列的第 10 章。请参阅此链接以获取该系列的其他章节 – [Golang 综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程** –  [For Range 循环](https://golangbyexample.com/understand-for-range-loop-golang/)

**前一个教程** – [常量](https://golangbyexample.com/constant-golang/)

现在让我们查看当前教程。以下是当前教程的目录。

# **概述**

在循环方面，Golang 具有：

+   for 循环

+   for-range 循环

在本教程中，我们将只学习 for 循环。while 循环在 Go 中缺失，但可以使用 for 循环来实现 while 循环，稍后我们将在本教程中看到。

**for** 循环在 GO 中基本上有三个部分，如下格式所示

+   初始化部分

+   条件部分

+   后置部分

```
for init_part; condition_part; post_part {
   ...
}
```

以下是三个部分的执行顺序：

+   **初始化部分**在第一次迭代前首先执行。

+   **条件部分**在每次迭代前执行。如果条件为假，循环将退出，否则循环将继续迭代。

+   **后置部分**在每次迭代后执行。之后检查条件，如果为真，则循环继续，否则循环结束。

关于 for 循环需要注意的一些要点：

+   在 for 循环中，括号不是必需的，但主体周围的花括号是必要的。

+   **初始化**和**后置**部分是可选的。

+   **初始化**部分可以是任何带有短声明、函数调用或赋值的语句。如果**初始化**部分有变量声明，则该变量的作用域仅限于 for 循环内部。

+   后置部分可以是任何语句，但通常包含增量逻辑。后置部分不能包含初始化。如果我们在后置部分添加任何初始化逻辑，编译器将会报错。

# **示例**

## **简单的 for 循环**

```
package main

import "fmt"

func main() {
    for i := 0; i < 5; i++ {
        fmt.Println(i)
    }
}
```

**输出：**

```
0
1
2
3
4
```

## **仅带条件的 for 循环**

```
package main

import "fmt"

func main() {
    i := 0
    for i < 5 {
        fmt.Println(i)
        i++
    }
}
```

**输出：**

```
0
1
2
3
4
```

## **无限循环**

```
package main

import (
    "fmt"
    "time"
)

func main() {
    i := 0
    for {
        fmt.Println(i)
        i++
        time.Sleep(time.Second * 1)
    }
}
```

**输出：** 上述程序打印一个无限循环

```
0
1
2
3
4
5
.
.
```

## **For 循环中的中断语句**

**break** 语句帮助退出 for 循环。循环内 break 后的语句不被执行。

```
package main

import (
    "fmt"
)

func main() {
    i := 0
    for {
        fmt.Println(i)
        i++
        if i >= 5 {
            break
        }
    }
}
```

**输出：**

```
0
1
2
3
4
```

## **For 循环中的继续语句**

**continue** 语句帮助跳过 for 循环的当前迭代。continue 后的语句不被执行，执行将重新回到开始，并进行下一次迭代。使用场景是当你只想对 for 循环的某些元素进行操作时。

让我们看看下面程序的例子，我们只想打印非 3 的倍数。

```
package main

import "fmt"

func main() {
    for i := 1; i < 10; i++ {
        if i%3 == 0 {
            continue
        }
        fmt.Println(i)
    }
}
```

**输出：**

```
1
2
4
5
7
8
10
```

## **嵌套 For 循环**

for 循环也可以嵌套。请看下面的例子。

```
package main

import "fmt"

func main() {
    for i := 0; i < 3; i++ {
        fmt.Printf("Outer loop iteration %d\n", i)
        for j := 0; j < 2; j++ {
            fmt.Printf("i= %d j=%d\n", i, j)
        }
    }
}
```

**输出：**

```
Outer loop iteration 0
i= 0 j=0
i= 0 j=1
Outer loop iteration 1
i= 1 j=0
i= 1 j=1
Outer loop iteration 2
i= 2 j=0
i= 2 j=1
```

## **初始化部分中的函数调用和赋值**

下面的代码是一个示例，我们也可以在初始化部分进行函数调用或赋值。

```
package main

import "fmt"

func main() {
    i := 1
    //Function call in the init part in for loop
    for test(); i < 3; i++ {
        fmt.Println(i)
    }

    //Assignment in the init part in for loop
    for i = 2; i < 3; i++ {
        fmt.Println(i)
    }
}
func test() {
    fmt.Println("In test function")
}
```

**输出**

```
In test function
1
2
2
```

## **使用for循环实现while循环**

Go没有**while**关键字。相反，它只有**for**关键字。然而，**for**关键字可以用来模拟与**while**相同的功能。

如果可以跳过**初始化部分**和**增量部分**，则可以将**for**循环实现为与**while**相同的行为。以下是一个示例：

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

# **结论**

这都是关于golang中的for循环。请在评论中分享反馈/改进/错误。

**下一教程** – [范围循环](https://golangbyexample.com/understand-for-range-loop-golang/)

**上一篇教程** – [常量](https://golangbyexample.com/constant-golang/)

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/)
