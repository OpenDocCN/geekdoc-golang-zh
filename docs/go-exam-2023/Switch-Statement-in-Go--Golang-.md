<!--yml

分类：未分类

日期：2024-10-13 06:18:12

-->

# Go（Golang）中的 Switch 语句。

> 来源：[https://golangbyexample.com/switch-statement-golang/](https://golangbyexample.com/switch-statement-golang/)

这是 golang 综合教程系列的第 13 章。有关系列的其他章节，请参考这个链接 – [Golang 综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程** – [延迟关键字](https://golangbyexample.com/defer-golang)

**上一个教程** – [如果则](https://golangbyexample.com/understand-if-else-statement-golang/)

现在让我们查看当前的教程。以下是当前教程的目录。

# **概述**

Switch 语句是防止 if-else 梯形结构的完美方法。以下是 switch 语句的格式。

```go
switch statement; expression {
case expression1:
     //Dosomething
case expression2:
     //Dosomething
default:
     //Dosomething
}
```

这就是 switch 的工作方式。给定一个 **switch 表达式**，它会遍历所有 case，尝试找到第一个匹配的 **case 表达式**，否则如果存在则执行默认 case。匹配的顺序是从上到下，然后从左到右（当 case 包含多个表达式时，如我们将在本教程后面看到的）。

# **重要事项**

在我们进入代码示例之前，有一些关于 switch 的重要信息需要了解。

+   **switch 语句** 和 **switch 表达式** 都是可选的语句。因此，它们存在四种可能的情况。

同时存在 **switch 语句** 和 **switch 表达式**。

```go
switch statement; expression {
   ... 
}
```

只有 **switch 语句**。请注意下面的语法。在 **switch 语句**后面需要有分号。

```go
switch statement; {
    ...
}
```

只有 **switch 表达式**。请注意下面的语法。在 **switch 表达式**后面没有分号。

```go
switch expression {
   ..
}
```

**switch 语句** 和 **switch 表达式** 都缺失。

```go
switch {
  ... 
}
```

+   如果未提供 **switch 表达式**，则编译器假定的默认类型是 **布尔**。

+   **switch 表达式** 的类型和所有 **case 表达式** 的类型应该匹配，否则会产生编译错误。当 **switch 表达式** 未提供时，所有 **case 表达式** 的类型也需要是布尔值。

+   **switch 语句** 可以是带有短声明、函数调用或赋值的任何语句。如果 **switch 语句** 包含变量声明，则该变量的作用域仅限于 switch 块。

+   它可以有任意数量的 case 语句。

+   默认 case 是可选的。

+   case 可以有多个用逗号分隔的表达式。然后 case 的样子如下。

```go
case expression1_1, expression_2 ...:
```

+   case 块还允许使用 **fallthrough** 关键字，该关键字将控制权转移到下一个 case，即使当前 case 可能已经匹配。

+   在许多其他语言中需要的 break 语句在 Go 中是不需要的。Go 自动在每个 case 块的末尾提供 break 语句。然而，显式使用 **break** 关键字来终止 switch 语句的执行也是正确的。

+   两个 case 不能有相同的常量值。在这种情况下会产生编译错误。

+   `switch` 语句也可以用作类型开关，用于在运行时知道空接口的类型，正如我们在下面的示例中看到的。

# **示例**

让我们看一些简单的示例来说明上述要点。

## **缺少 `switch` 语句和 `switch` 表达式**

```go
package main

import "fmt"

func main() {
    switch ch := "b"; ch {
    case "a":
        fmt.Println("a")
    case "b", "c":
        fmt.Println("b or c")    
    default:
        fmt.Println("No matching character")    
    }

    //fmt.Println(ch)

} 
```

**输出：**

```go
b or c
```

一些注意事项：

+   在上述示例中，我们有一个带有短声明的 `switch` 语句。

+   然后，switch case 匹配了这里的 `switch` 表达式“b”。

+   此外，case 中可以有多个表达式，如上所示，第二个 case 有两个表达式。

```go
 case "b", "c":
```

+   **ch** 变量仅在 **switch** 块内可用。取消注释位于 `switch` 块外的 **fmt.Println(ch)** 行，它将引发错误。

```go
undefined: ch
```

## **缺少 `switch` 语句和 `switch` 表达式**

让我们看另一个示例，其中我们省略了 **switch 语句** 和 **switch 表达式**。

```go
package main

import "fmt"

func main() {
    age := 45
    switch {
    case age < 18:
        fmt.Println("Kid")
    case age >= 18 && age < 40:
        fmt.Println("Young")
    default:
        fmt.Println("Old")
    }
}
```

**输出**

```go
Old
```

关于上述示例有几点需要注意：

+   由于我们省略了 `switch` 表达式，`switch` 表达式的默认类型为布尔值。每个 case 表达式也评估为布尔值，因此程序运行正常。

+   默认 case 在上述示例中执行，因为没有任何 case 表达式匹配。

## **仅 `switch` 语句**

注意语句后面的 **';'**

```go
package main

import "fmt"

func main() {

    switch age := 29; {
    case age < 18:
        fmt.Println("Kid")
    case age >= 18 && age < 40:
        fmt.Println("Young")
    default:
        fmt.Println("Old")
    }
}
```

**输出：**

```go
Young
```

## **仅 `switch` 表达式**

```go
package main

import "fmt"

func main() {
    char := "a"
    switch char {
    case "a":
        fmt.Println("a")
    case "b":
        fmt.Println("b")
    default:
        fmt.Println("No matching character")
    }
}
```

**输出**

```go
a
```

## **重复的 case**

两个 case 语句不能有相同的常量。例如，在下面的 case 中，会出现编译错误，因为 "a" 在两个 case 中都存在。

```go
duplicate case "a" in switch
```

```go
package main

import "fmt"

func main() {
    switch "a" {
    case "a":
        fmt.Println("a")
    case "a":
        fmt.Println("Another a")
    case "b":
        fmt.Println("b")
    default:
        fmt.Println("No matching character")
    }
}
```

## **Fallthrough 关键字**

请查看下面的代码以了解 **fallthrough** 关键字的示例。在下面的示例中，尽管第二个 case 匹配，但由于 **fallthrough** 关键字，它进入了第三个 case。

```go
package main

import "fmt"

func main() {
    i := 45
    switch {
    case i < 10:
        fmt.Println("i is less than 10")
        fallthrough
    case i < 50:
        fmt.Println("i is less than 50")
        fallthrough
    case i < 100:
        fmt.Println("i is less than 100")
    }
}
```

**输出**

```go
i is less than 50
i is less than 100
```

**fallthrough** 必须是 `switch` 块内的最后一条语句。如果不是，编译器将引发错误。

```go
fallthrough statement out of place
```

下面的程序将在 **fallthrough** 语句之后有 **fmt.Println** 时引发上述错误。

```go
package main

import "fmt"

func main() {
    i := 45
    switch {
    case i < 10:
        fmt.Println("i is less than 10")
        fallthrough
    case i < 50:
        fmt.Println("i is less than 50")
        fallthrough
        fmt.Println("Not allowed")
    case i < 100:
        fmt.Println("i is less than 100")
    }
}
```

## **Break 语句**

下面是 **break** 语句示例。

```go
package main

import "fmt"

func main() {
    switch char := "b"; char {
    case "a":
        fmt.Println("a")
    case "b":
        fmt.Println("b")
        break
        fmt.Println("after b")
    default:
        fmt.Println("No matching character")
    }
}
```

**输出**

```go
b
```

**break** 语句将终止 `switch` 的执行，下面的行将永远不会执行。

```go
fmt.Println("after b")
```

## **类型开关**

`switch` 语句也可以用于在运行时知道接口的类型，如下面示例所示。类型开关比较类型而不是值。

```go
package main

import "fmt"

func main() {
    printType("test_string")
    printType(2)
}

func printType(t interface{}) {
    switch v := t.(type) {
    case string:
        fmt.Println("Type: string")
    case int:
        fmt.Println("Type: int")
    default:
        fmt.Printf("Unknown Type %T", v)
    }
}
```

**输出：**

```go
Type: string
Type: int
```

# **结论**

这就是关于 Go 中 `switch` 语句的全部内容。希望你喜欢这篇文章。请在评论中分享反馈/改进/错误。

**下一个教程** – [Defer 关键字](https://golangbyexample.com/defer-golang)

**上一个教程** – [If Else](https://golangbyexample.com/understand-if-else-statement-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [switch](https://golangbyexample.com/tag/switch/)
