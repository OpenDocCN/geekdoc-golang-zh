<!--yml
category: 未分类
date: 2024-10-13 06:22:16
-->

# Benefits of Interface in Go (Golang)

> 来源：[https://golangbyexample.com/benefits-of-interface-golang/](https://golangbyexample.com/benefits-of-interface-golang/)

Below are some benefits of using interface.

*   Helps write more modular and decoupled code between different parts of the codebase – It can help reduce dependency between different parts of the codebase and provide loose coupling.

For eg imagine an application interacting with a database layer. If the application interacts with the database using the interface, then it never gets to know about what kind of database is being used in the background. You can change the type of database in the background, let’s say from arango db to mongo db without any change in the application layer as it interacts with the database layer via an interface which both arango db and mongo db implement.

*   Interface can be used to achieve run time polymorphism in golang. RunTime Polymorphism means that a call is resolved at runtime. Let’s understand how an interface can be used to achieve runtime polymorphism with an example

Different countries have different ways of calculating the tax. This can be represented by means of an interface.

```
type taxCalculator interface{
    calculateTax()
}
```

Now different countries can have their own struct and can implement the **calculateTax()** method. The same **calculateTax** method is used in different contexts to calculate tax. When the compiler sees this call it delays which exact method to be called at run time.

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

*   [go](https://golangbyexample.com/tag/go/)