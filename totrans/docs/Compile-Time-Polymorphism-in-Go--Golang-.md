<!--yml
category: 未分类
date: 2024-10-13 06:01:24
-->

# Compile Time Polymorphism in Go (Golang)

> 来源：[https://golangbyexample.com/compile-time-polymorphism-go/](https://golangbyexample.com/compile-time-polymorphism-go/)

In compile-time polymorphism, the call is resolved during compile time by the compiler. Some of the  forms for compile-time polymorphism are

*   **Method/Function Overloading**: more than one method/function exists with the same name but with different signatures or possibly different return types
*   **Operator Overloading**: the Same operator is used for operating on different data types

Go doesn’t support Method Overloading. For example, see below program demonstrating that go doesn’t support method overloading.

```
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

**Output:**

```
(*maths).add redeclared in this block
        previous declaration at ./main.go:5:6
```

Go also doesn’t support operator overloading. Reason for this is stated in faq of go – [https://golang.org/doc/faq#overloading](https://golang.org/doc/faq#overloading)

Method dispatch is simplified if it doesn’t need to do type matching as well. Experience with other languages told us that having a variety of methods with the same name but different signatures was occasionally useful but that it could also be confusing and fragile in practice. Matching only by name and requiring consistency in the types was a major simplifying decision in Go’s type system.
Regarding operator overloading, it seems more a convenience than an absolute requirement. Again, things are simpler without it.

Now the question is there any alternative to do method overloading in GO. This is where **Variadic function** in go comes into the picture. See below program

```
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

**Output:**

```
Result: 5
Result: 9
```

**Conclusion**:

Go doesn’t directly support method/function/operator overloading but variadic function provides a way of achieving the same with increased code complexity.

*   [compile](https://golangbyexample.com/tag/compile/)*   [compile-polymorphism](https://golangbyexample.com/tag/compile-polymorphism/)*   [oop](https://golangbyexample.com/tag/oop/)*   [polymorphism](https://golangbyexample.com/tag/polymorphism/)