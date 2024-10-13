<!--yml

分类：未分类

日期：2024-10-13 06:20:20

-->

# Go（Golang）中的方法链

> 来源：[`golangbyexample.com/method-chaining-go/`](https://golangbyexample.com/method-chaining-go/)

为了实现方法链的功能，链中的方法应该返回接收者。链中最后一个方法返回接收者是可选的。

让我们看看方法链的一个示例。

```go
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

**输出**

```go
Name: Sam
Age: 31
Salary: 2000
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)
