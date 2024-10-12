<!--yml
category: 未分类
date: 2024-10-13 06:01:29
-->

# Runtime Polymorphism in Go (Golang)

> 来源：[https://golangbyexample.com/runtime-polymorphism-go/](https://golangbyexample.com/runtime-polymorphism-go/)

RunTime Polymorphism means that a call is resolved at runtime. It is achieved in GO using interfaces. 

Let’s understand it with an example. Different countries have different ways of calculating the tax. This can be represented by means of an interface.

```
type taxCalculator interface{
    calculateTax()
}
```

Now different countries can have their own struct and can implement the **calculateTax()** method. For eg for **indianTax** struct can be represented as below. It can also define a method calculateTax() which can do the actual calculation based on percentage.

Similarly, other country’s tax systems can also be represented by a struct and they can also implement their own calculateTax() method to give their own value of tax. 

Now let’s see how we can use this **taxCalcuator **in****terface to calculate tax of a person who is settled in different countries in different parts of the year. See the below full program for reference.

```
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

**Output:**

```
Total Tax is 300
```

Now below is the line where run time polymorphism happens. 

```
 totalTax += t.calculateTax() //This is where runtime polymorphism happens
```

The same **calculateTax** is used in different contexts to calculate tax. When the compiler sees this call it delays which exact method to be called at run time. This magic happens behind the scene. If you are interested in knowing how this magic happens then refer to this link.

**Extension to add more tax systems:**

Now let’s extend the above program to also include the tax system for USA countries

```
type usaTax struct {
    taxPercentage int
    income        int
}

func (i *usaTax) calculateTax() int {
    tax := i.income * i.taxPercentage / 100
    return tax
}
```

We just have to change our main function to add USA tax system.

```
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

**Output:**

```
Total Tax is 700
```

If you notice in the above program **calculateTotalTax** function doesn’t need to be changed to accommodate the USA tax system. That is the benefit of interfaces and polymorphism.