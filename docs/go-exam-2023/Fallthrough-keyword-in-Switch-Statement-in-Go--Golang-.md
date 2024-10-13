<!--yml

category: 未分类

date: 2024-10-13 06:21:12

-->

# Go (Golang)中的 Switch 语句中的 Fallthrough 关键字

> 来源：[`golangbyexample.com/fallthrough-keyword-golang/`](https://golangbyexample.com/fallthrough-keyword-golang/)

**fallthrough**关键字在 golang 的 switch 语句中使用。该关键字用于 switch case 块。如果 case 块中存在**fallthrough**关键字，则即使当前 case 可能已匹配，它也会将控制权转移到下一个 case。

为了更好地理解这一点，让我们先看一个没有 fallthrough 关键字的示例。

```go
package main

import "fmt"

func main() {
    i := 45

    switch {
    case i < 10:
        fmt.Println("i is less than 10")
    case i < 50:
        fmt.Println("i is less than 50")
    case i < 100:
        fmt.Println("i is less than 100")
    }
}
```

**输出：**

```go
i is less than 50
```

默认情况下，switch 语句从上到下匹配所有 case 语句，并尝试找到第一个与**switch 表达式**匹配的**case 表达式**。一旦找到匹配的 case，它将退出，不考虑其他 case。这就是上述示例中发生的情况。尽管 i 小于 100，但该 case 永远不会被执行，因为匹配了第二个 case，之后它退出。

fallthrough 关键字提供了一种绕过此限制的方法。请参见下面的代码，了解**fallthrough**关键字的示例。在下面的示例中，尽管第二个 case 已匹配，但由于**fallthrough**关键字，它仍然经过了第三个 case。

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

**fallthrough**必须是 switch 块内的最后一个语句。如果不是，编译器将引发错误。

```go
fallthrough statement out of place
```

以下程序将引发上述错误，因为我们在**fmt.Println**语句之后有**fallthrough**语句。

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

**Break 语句**

以下是**break**语句的示例。

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

**break**语句将终止 switch 的执行，下面的行将永远不会被执行。

```go
fmt.Println("after b")
```

**结论**

这就是关于 golang 中 switch 语句中 fallthrough 关键字的所有内容。

+   [fallthrough](https://golangbyexample.com/tag/fallthrough/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)
