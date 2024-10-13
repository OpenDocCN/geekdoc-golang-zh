<!--yml

分类：未分类

日期：2024-10-13 06:18:07

-->

# 理解Go中的如果-否则语句（Golang）

> 来源：[https://golangbyexample.com/understand-if-else-statement-golang/](https://golangbyexample.com/understand-if-else-statement-golang/)

这是golang综合教程系列的第12章。有关该系列其他章节，请参考此链接——[Golang综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程**——[开关](https://golangbyexample.com/switch-statement-golang/)

**上一个教程**——[范围循环](https://golangbyexample.com/understand-for-range-loop-golang/)

现在让我们查看当前教程。以下是当前教程的目录。

# **概述**

Go有类似于其他编程语言的if-else语句，以执行基本的条件逻辑。以下是golang中if-else语句的格式。

```go
if condition {
   //Do something
} else if condition {
   //Do something
} else {
   //Do something
```

在我们进一步讨论之前，先谈谈**条件**。条件中只允许语句或结果为布尔值的语句组合。在Go中，false布尔值在条件中被视为假，true布尔值被视为真。如上所述，条件可以由多个语句通过运算符组合而成，例如&&、||、>、<、>=、<=、!等。

现在让我们详细看看if-else语句，以理解其中的小细节。Go支持以下格式的if-else语句。

+   仅仅if

+   如果-否则

+   如果-否则阶梯

+   嵌套if-else

+   如果与简短语句

## **如果语句**

if语句单独具有以下格式。

```go
if condition {
   //Do something
}
```

如果条件为真，则执行括号内的语句。关于if语句需要注意的一些要点。

+   可以省略条件周围的括号。

+   条件后的开闭括号是强制的。

让我们看一个工作示例。以下程序检查一个数字是否大于5。

```go
package main

import "fmt"

func main() {
    a := 6
    if a > 5 {
        fmt.Println("a is greater than 5")
    }
}
```

**输出**

```go
a is greater than 5
```

让我们看另一个if条件中的多个语句的示例。以下是一个程序，用于检查一个数字是否在特定范围内。注意条件中的多个语句由&&运算符连接。

```go
package main

import "fmt"

func main() {
    a := 4
    if a > 3 && a < 6 {
        fmt.Println("a is within range")
    }
}
```

**输出**

```go
a is within range
```

## **如果-否则语句**

如果-否则语句具有以下格式。

```go
if condition {
   //Do something
} else {
   //Do something
}
```

如果条件为真，则执行if块中的语句，否则执行else块中的语句。关于if-else语句需要注意一些要点。

+   else关键字应该与其闭合括号在同一行。如果不在同一行，将会出现以下编译器错误。

```go
syntax error: unexpected else, expecting }
```

让我们看一个小的if-else语句示例。在下面的程序中，我们使用if-else语句找出两个数字中的最大值。

```go
package main

import "fmt"

func main() {
    a := 1
    b := 2

    if a > b {
        fmt.Println("a is greater than b")
    } else {
        fmt.Println("b is greater than a")
    }
}
```

**输出**

```go
b is greater than a
```

## **如果-否则阶梯**

如果-否则阶梯具有以下格式。

```go
if condition1 {
   //Do something
} else if condition2 {
   //Do something
} else {
  //Do something
}
```

关于这个if-else阶梯需要注意的一些要点。

+   可以在中间添加任意数量的else if语句。

+   else if应位于前一个闭合括号的同一行。

以下是一个可工作的代码示例。该代码根据年龄使用if-else阶梯来判断一个人是“孩子”、“年轻”还是“老年”。

```go
package main

import "fmt"

func main() {
    age := 29
    if age < 18 {
        fmt.Println("Kid")
    } else if age >= 18 && age < 40 {
        fmt.Println("Young")
    } else {
        fmt.Println("Old")
    }
}
```

**输出：**

```go
Young
```

## **嵌套如果否则**

下面是嵌套 if else 的一些可能格式。

仅嵌套 if

```go
if condition {
  //Do something
  if condition2 { 
    //Do something
  }
  //Do something
}
```

嵌套 if else

```go
if condition1 {
   //....
   if condition2 {
      //...
   } else {
      //...
   }
  //...
}
```

下面的组合也可以用于嵌套的 if else。

```go
if condition1 {
   //...
} else {
   //...
   if condition2 {
      //...
   } else {
     //....
   }
   //....
}
```

让我们看一个嵌套 if else 的工作示例。在下面的程序中，我们使用嵌套 if else 打印三个数字中的最大值。

```go
package main

import "fmt"

func main() {
    a := 1
    b := 2
    c := 3
    if a > b {
        if a > c {
            fmt.Println("Biggest is a")
        } else if b > c {
            fmt.Println("Biggest is b")
        }
    } else if b > c {
        fmt.Println("Biggest is b")
    } else {
        fmt.Println("Biggest is c")
    }
}
```

**输出：**

```go
Biggest is c
```

## **带短语句的 if**

If 语句也支持在条件前添加语句。这个语句将在条件之前执行。语句中还可以有新的初始化变量。下面是该格式。

```go
if statement; condition {
   //Do something
}
```

如果语句中存在初始化，它将是一个短声明。注意，在语句中不支持 var 关键字。让我们看一个工作示例。

```go
package main

import "fmt"

func main() {
    if a := 6; a > 5 {
        fmt.Println("a is greater than 5")
    }
}
```

**输出**

```go
a is greater than 5
```

在 if 语句中初始化的变量在所有分支中都可用。如下面示例中，变量 **a** 在 else 块中也可用。

```go
package main

import "fmt"

func main() {
    if a := 1; a > 5 {
        fmt.Println("a is greater than 5")
    } else {
        fmt.Println("a is less than 5")
    }
}
```

**输出：**

```go
a is less than 5
```

# **if 条件**

我们在开头提到过，if 条件中只允许布尔值或结果为布尔值的语句。让我们看看使用非布尔值时出现的错误的工作代码。

```go
package main

import "fmt"

func main() {
    if 1 {
        fmt.Println("a is greater than 5")
    }
}
```

**输出：** 下面的编译器错误被抛出

```go
non-bool 1 (type int) used as if condition
```

# **三元运算符**

Go 中没有三元运算符，因此你需要使用 if else 语句来替代它。

# **结论**

这就是 Go 语言中 if else 语句的全部内容。希望你喜欢这篇文章。请在评论中分享反馈/改进/错误。

**下一个教程** – [Switch](https://golangbyexample.com/switch-statement-golang/)

**上一个教程** – [范围循环](https://golangbyexample.com/understand-for-range-loop-golang/)

+   [完整](https://golangbyexample.com/tag/complete/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [指南](https://golangbyexample.com/tag/guide/)*   [理解](https://golangbyexample.com/tag/understand/)*   [使用](https://golangbyexample.com/tag/using/)
