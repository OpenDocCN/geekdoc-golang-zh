<!--yml

分类：未分类

日期：2024-10-13 06:01:29

-->

# Go中的运行时多态（Golang）

> 来源：[https://golangbyexample.com/runtime-polymorphism-go/](https://golangbyexample.com/runtime-polymorphism-go/)

运行时多态意味着一个调用在运行时被解析。它在GO中通过使用接口来实现。

让我们通过一个例子来理解。不同国家有不同的计算税的方法。这可以通过接口来表示。

```go
type taxCalculator interface{
    calculateTax()
}
```

现在不同国家可以拥有自己的结构体，并且可以实现**calculateTax()**方法。例如，**indianTax**结构可以表示如下。它还可以定义一个方法calculateTax()，根据百分比进行实际计算。

类似地，其他国家的税制也可以通过结构体表示，并且它们也可以实现自己的calculateTax()方法，以提供自己的税值。

现在让我们看看如何使用这个**taxCalculator**接口来计算一个人在不同国家的税务，尤其是在一年中的不同时间。请参见下面的完整程序作为参考。

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
type usaTax struct {
    taxPercentage int
    income        int
}
func (i *usaTax) calculateTax() int {
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

下面是运行时多态发生的地方。

```go
 totalTax += t.calculateTax() //This is where runtime polymorphism happens
```

同样的**calculateTax**在不同的上下文中用于计算税。当编译器看到这个调用时，它会延迟决定在运行时调用哪个具体方法。这种魔法在幕后发生。如果你想知道这种魔法是如何发生的，请参考这个链接。

**扩展以添加更多税制：**

现在让我们扩展上述程序，以包括美国的税制。

```go
type usaTax struct {
    taxPercentage int
    income        int
}

func (i *usaTax) calculateTax() int {
    tax := i.income * i.taxPercentage / 100
    return tax
}
```

我们只需更改主函数以添加美国税制。

```go
func main() {
    indianTax := &indianTax{
        taxPercentage: 30,
        income:        1000,
    }
    singaporeTax := &singaporeTax{
        taxPercentage: 10,
        income:        2000,
    }
    usaTax := &usaTax{
        taxPercentage: 40,
        income:        500,
    }

    taxSystems := []taxSystem{indianTax, singaporeTax, usaTax}
    totalTax := calculateTotalTax(taxSystems)
    fmt.Printf("Total Tax is %d\n", totalTax)
}
func calculateTotalTax(taxSystems []taxSystem) int {
    totalTax := 0
    for _, t := range taxSystems {
        totalTax += t.calculateTax()
    }
    return totalTax
}
```

**输出：**

```go
Total Tax is 700
```

如果你注意到上面的程序，**calculateTotalTax**函数无需更改以适应美国税制。这就是接口和多态的好处。
