<!--yml

类别：未分类

日期：2024-10-13 06:01:24

-->

# Go（Golang）中的编译时多态性

> 来源：[https://golangbyexample.com/compile-time-polymorphism-go/](https://golangbyexample.com/compile-time-polymorphism-go/)

在编译时多态中，调用在编译时由编译器解析。编译时多态的一些形式是

+   **方法/函数重载**：存在多个同名但具有不同签名或可能不同返回类型的方法/函数。

+   **运算符重载**：相同的运算符用于不同数据类型的操作。

Go 不支持方法重载。例如，以下程序演示了 Go 不支持方法重载。

```go
package main

type maths struct{}

func (m *maths) add(a, b int) int {
    return a + b
}

func (m *maths) add(a, b, c int) int {
    return a + b + c
}

func main() {
   m := &maths{}
}
```

**输出：**

```go
(*maths).add redeclared in this block
        previous declaration at ./main.go:5:6
```

Go 也不支持运算符重载。原因在于 Go 的常见问题解答中说明了——[https://golang.org/doc/faq#overloading](https://golang.org/doc/faq#overloading)。

如果方法分发不需要进行类型匹配，则会简化。与其他语言的经验告诉我们，拥有多个同名但具有不同签名的方法有时是有用的，但在实践中也可能令人困惑且脆弱。在 Go 的类型系统中，仅通过名称匹配并要求类型一致性是一个重要的简化决策。

关于运算符重载，它似乎更多是一种便利，而非绝对的要求。没有它，事情更简单。

现在的问题是，Go 中是否有替代方法进行方法重载。这就是 Go 中的 **可变参数函数** 进入画面的地方。见以下程序。

```go
package main

import "fmt"

type maths struct{}

func (m *maths) add(numbers ...int) int {
    result := 0
    for _, num := range numbers {
        result += num
    }
    return result
}

func main() {
    m := &maths{}

    fmt.Printf("Result: %d\n", m.add(2, 3))
    fmt.Printf("Result: %d\n", m.add(2, 3, 4))
}
```

**输出：**

```go
Result: 5
Result: 9
```

**结论**：

Go 不直接支持方法/函数/运算符重载，但可变参数函数提供了一种实现相同功能的方法，尽管代码复杂性增加。

+   [编译](https://golangbyexample.com/tag/compile/)*   [编译-多态](https://golangbyexample.com/tag/compile-polymorphism/)*   [面向对象编程](https://golangbyexample.com/tag/oop/)*   [多态性](https://golangbyexample.com/tag/polymorphism/)
