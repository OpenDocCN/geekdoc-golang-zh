<!--yml
category: 未分类
date: 2024-10-13 06:20:20
-->

# Method Chaining in Go (Golang)

> 来源：[https://golangbyexample.com/method-chaining-go/](https://golangbyexample.com/method-chaining-go/)

For method chaining to be possible, the methods in the chain should return the receiver. Returning the receiver for the last method in the chain is optional.

Let’s see an example of method chaining.

```
package main

import "fmt"

type employee struct {
	name   string
	age    int
	salary int
}

func (e employee) printName() employee {
	fmt.Printf("Name: %s\n", e.name)
	return e
}

func (e employee) printAge() employee {
	fmt.Printf("Age: %d\n", e.age)
	return e
}

func (e employee) printSalary() {
	fmt.Printf("Salary: %d\n", e.salary)
}

func main() {
	emp := employee{name: "Sam", age: 31, salary: 2000}
	emp.printName().printAge().printSalary()
}
```

**Output**

```
Name: Sam
Age: 31
Salary: 2000
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)