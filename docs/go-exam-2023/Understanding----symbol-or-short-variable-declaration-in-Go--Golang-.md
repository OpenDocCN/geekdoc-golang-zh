<!--yml

类别：未分类

日期：2024-10-13 06:21:31

-->

# 理解 Go (Golang) 中的 := 符号或短变量声明。

> 来源：[`golangbyexample.com/short-variable-declaration-go/`](https://golangbyexample.com/short-variable-declaration-go/)

Go 提供了另一种声明变量的方法，即使用 **:=** 操作符。当使用 **:=** 操作符时，可以省略 **var** 关键字和类型信息。以下是这种声明的格式。

```go
{var_name} := {value}
```

类型推断将根据值的类型进行。同时，请参阅此文章以获取有关类型推断的详细信息。让我们看看一个工作示例。在下面的代码中。

+   **m2** 的类型被正确推断为 **int**，因为赋给它的值是 123，而 123 是 **int** 类型。

+   同样，n2 的类型也被正确推断为字符串，因为赋给它的值是 **“circle”**，它是一个 **string**，依此类推。

+   另外，请注意变量 **t2** 的类型被正确推断为结构体 **main.sample**。

+   **u2**的类型被正确推断为**[]string**，因为这是**get()**函数调用返回的类型。

```go
package main

import "fmt"

func main() {
    m2 := 123                   //Type Inferred will be int
    n2 := "circle"              //Type Inferred will be string
    o2 := 5.6                   //Type Inferred will be float64
    p2 := true                  //Type Inferred will be bool
    q2 := 'a'                   //Type Inferred will be rune
    r2 := 3 + 5i                //Type Inferred will be complex128
    s2 := &sample{name: "test"} //Type Inferred will be pointer to main.sample
    t2 := sample{name: "test"}  //Type Inferred will be main.sample
    u2 := get()                 //Type inferred will []string as that is the return value of function get()

    fmt.Println("")
    fmt.Printf("m2: Type: %T Value: %v\n", m2, m2)
    fmt.Printf("n2: Type: %T Value: %v\n", n2, n2)
    fmt.Printf("o2: Type: %T Value: %v\n", o2, o2)
    fmt.Printf("p2: Type: %T Value: %v\n", p2, p2)
    fmt.Printf("q2: Type: %T Value: %v\n", q2, q2)
    fmt.Printf("r2: Type: %T Value: %v\n", r2, r2)
    fmt.Printf("s2: Type: %T Value: %v\n", s2, s2)
    fmt.Printf("t2: Type: %T Value: %v\n", t2, t2)
    fmt.Printf("u2: Type: %T Value: %v\n", u2, u2)
}

type sample struct {
    name string
}

func get() []string {
    return []string{"a", "b"}
}
```

**输出**

```go
m2: Type: int Value: 123
n2: Type: string Value: circle
o2: Type: float64 Value: 5.6
p2: Type: bool Value: true
q2: Type: int32 Value: 97
r2: Type: complex128 Value: (3+5i)
s2: Type: *main.sample Value: &{test}
t2: Type: main.sample Value: {test}
u2: Type: []string Value: [a b]
```

关于 := 操作符需要注意的一些要点。

+   := 操作符仅在函数内部可用，函数外部不允许使用。

+   使用 := 声明的变量一旦被声明，就不能再次使用 := 操作符进行重新声明。因此，下面的语句将引发编译器错误“**左侧没有新变量在 :=**”。

```go
a := 8
a := 16
```

+   := 操作符还可以用于在一行中声明多个变量。请参见下面的示例。

```go
a,b := 1, 2
```

+   在多个声明的情况下，如果左侧至少有一个变量是新的，则可以再次使用 := 来声明特定变量。请参见下面的示例。注意，b 再次使用 := 被声明。这只有在至少有一个变量是新的（这里是变量 **c**）的情况下才可能。在这种情况下，它作为变量 **b** 的赋值。

```go
package main

import "fmt"

func main() {
    a, b := 1, 2
    b, c := 3, 4
    fmt.Println(a, b, c)
}
```

**输出：**

```go
1, 3, 4
```

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/)
