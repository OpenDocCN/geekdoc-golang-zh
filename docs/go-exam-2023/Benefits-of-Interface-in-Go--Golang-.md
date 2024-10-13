<!--yml

分类：未分类

日期：2024-10-13 06:22:16

-->

# Go (Golang) 中接口的好处

> 来源：[https://golangbyexample.com/benefits-of-interface-golang/](https://golangbyexample.com/benefits-of-interface-golang/)

以下是使用接口的一些好处。

+   有助于在代码库的不同部分之间编写更模块化和解耦的代码——这有助于减少代码库不同部分之间的依赖关系，并提供松耦合。

例如，想象一个与数据库层交互的应用程序。如果应用程序通过接口与数据库交互，那么它就不会知道后台使用的是哪种数据库。你可以在后台更改数据库类型，比如从 Arango DB 更改为 Mongo DB，而无需对应用层进行任何更改，因为它是通过实现了接口的数据库层与数据库交互的。

+   接口可以用于实现 Golang 中的运行时多态性。运行时多态性意味着调用在运行时解析。让我们通过一个示例理解接口如何用于实现运行时多态性。

不同国家有不同的税收计算方式。这可以通过接口来表示。

```go
type taxCalculator interface{
    calculateTax()
}
```

现在，不同国家可以拥有自己的结构并实现 **calculateTax()** 方法。相同的 **calculateTax** 方法在不同的上下文中用于计算税款。当编译器看到这个调用时，它会推迟在运行时调用哪个具体的方法。

```go
package main

import "fmt"

type taxSystem interface {
    calculateTax() int
}
type indianTax struct {
    taxPercentage int
    income        int
}
func (i *indianTax) calculateTax() int {
    tax := i.income * i.taxPercentage / 100
    return tax
}
type singaporeTax struct {
    taxPercentage int
    income        int
}
func (i *singaporeTax) calculateTax() int {
    tax := i.income * i.taxPercentage / 100
    return tax
}

func main() {
    indianTax := &indianTax{
        taxPercentage: 30,
        income:        1000,
    }
    singaporeTax := &singaporeTax{
        taxPercentage: 10,
        income:        2000,
    }

    taxSystems := []taxSystem{indianTax, singaporeTax}
    totalTax := calculateTotalTax(taxSystems)

    fmt.Printf("Total Tax is %d\n", totalTax)
}
func calculateTotalTax(taxSystems []taxSystem) int {
    totalTax := 0
    for _, t := range taxSystems {
        totalTax += t.calculateTax() //This is where runtime polymorphism happens
    }
    return totalTax
}
```

**输出：**

```go
Total Tax is 300
```

+   [go](https://golangbyexample.com/tag/go/)
