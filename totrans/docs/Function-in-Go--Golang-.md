<!--yml
category: 未分类
date: 2024-10-13 06:16:51
-->

# Function in Go (Golang)

> 来源：[https://golangbyexample.com/function-golang-complete-guide/](https://golangbyexample.com/function-golang-complete-guide/)

This is the  chapter 8 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [Constants](https://golangbyexample.com/constant-golang/)
**Previous Tutorial** –[All basic data types](https://golangbyexample.com/all-basic-data-types-golang/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Signature of a function](#Signature_of_a_function "Signature of a function")
*   [Calling a function](#Calling_a_function "Calling a function")
*   [Function Parameters](#Function_Parameters "Function Parameters")
*   [Return Values](#Return_Values "Return Values")
    *   [Named Return Values](#Named_Return_Values "Named Return Values")
*   [Function Usages](#Function_Usages "Function Usages")
    *   [Generic Usage](#Generic_Usage "Generic Usage")
    *   [Function as  Type](#Function_as_Type "Function as  Type")
        *   [Function as User Defined Type](#Function_as_User_Defined_Type "Function as User Defined Type")
    *   [Function as values (or Anonymous functions)](#Function_as_values_or_Anonymous_functions "Function as values (or Anonymous functions)")
*   [Special Usages of Function](#Special_Usages_of_Function "Special Usages of Function")
    *   [Function Closures](#Function_Closures "Function Closures")
    *   [Higher Order Function](#Higher_Order_Function "Higher Order Function")
    *   [IIF or Immediately Invoked Function](#IIF_or_Immediately_Invoked_Function "IIF or Immediately Invoked Function")
    *   [Variadic Function](#Variadic_Function "Variadic Function")
    *   [Methods](#Methods "Methods")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

A function is a group of statements that perform a specific task. In GO functions are first-order variables. They can be passed around like any other variable. 

Some point worth noting about a function name

*   A function name cannot begin with a number

*   The function name is case sensitive. Hence sum, Sum,  SUM are different functions. 

*   A function whose name starts with a capital letter will be exported outside its package and can be called from other packages. A function whose name starts with lowercase letters will not be exported and it’s only visible within its package.

# **Signature of a function**

```
func func_name(input_parameters) return_values{
  //body
}
```

A function in golang

*   Declared using **func** keyword
*   It has a name
*   Comma-separated zero or more input parameters
*   Comma-separated zero or more return values
*   Function body
*   Can return multiple values

An example of a function. The below function is

*   Has the name **“sum”**
*   Accept two arguments of int type
*   Return a single value of type int

```
func sum(a int, b int) int {
  return a + b 
}
```

# **Calling a function**

A function in Go can be called in go as below

```
results := sum(2, 3)
```

Some points to note about calling a function

*   If the function of some other package is being called then it is necessary to prefix the package name. Also please note that across packages only those function can be called which are exported meaning whose name start with capital letter

*   With in the same package the function can directly be called using its name suffixed by ()

# **Function Parameters**

*   As mentioned above function can have zero or more arguments.

*   type for the consecutive same types can be specified only once. For example above sum function can also be written as

```
func sum(a, b int)
```

*   A copy of all the arguments is made while calling a function.

# **Return Values**

*   As mentioned above a function can have one or more return values. Assume there is a function sum_avg that returns two values: Sum and Average. Example of multiple return values

```
func sum_avg(a, b int) (int, int)
```

*   As a convention error is returned as the last argument in a function. Example

```
func sum(a, b int) (int, error)
```

*   Collecting multiple return values in the caller function. In below example

```
result, err := sum(2, 3) 
```

## **Named Return Values**

A go function can have named return values. With named return values , the return values did not need to be initialised in the function**.** The named variables are specified in the signature itself. Without named values,  only return type is specified. It is also ok to name some of the return values. For other return values only type can be specified. 

*   See example below: **result** is the named return value

```
func sum(a, b int) (result int)
```

*   With named return values, type of the consecutive same types can be specified only once

```
func sum_avg(a, b int) (sum, avg int)
```

# **Function Usages**

*   Generic usage
*   Function as Type
*   Function as Values

The difference between function as type and function as values is that in type we only use the function signature whereas in function as value signature along with the body is used.  Let’s see each of this in detail so it is more clear

## **Generic Usage**

Below is an example of the general usage of a function. We have a function sum in the below example which takes in two ints as input arguments and returns the sum.

```
package main

import "fmt"

func main() {
    res := sum(2, 3)
    fmt.Println(res)
}

func sum(a, b int) int {
    return a + b
}
```

**Output:**

```
5
```

## **Function as  Type**

In go function is also a type. Two function will be of same type if

*   They have the same number of arguments with each argument is of the same type

*   They have same number of return values and each return value is of same type. 

Function type is useful in 

*   In the case of higher-order functions as we have seen in the above example. The argument and return type is specified using function type
*   In the case of defining interfaces in go as in the interface, only the function type is specified. Whatever implements this interface has to define a function of the same type

Let’s see an example of function type in the interface. Notice that the interface **shape** only defines the type of function

```
area() int
getType() string
```

**Code:**

```
package main
import "fmt"
func main() {
    var shapes []shape
    s := &square{side: 2}
    shapes = append(shapes, s)
    r := &rectangle{length: 2, breath: 3}
    shapes = append(shapes, r)
    for _, shape := range shapes {
        fmt.Printf("Type: %s, Area %d\n", shape.getType(), shape.area())
    }
}
type shape interface {
    area() int
    getType() string
}
type rectangle struct {
    length int
    breath int
}
func (r *rectangle) area() int {
    return r.length * r.breath
}
func (r *rectangle) getType() string {
    return "rectangle"
}
type square struct {
    side int
}
func (s *square) area() int {
    return s.side * s.side
}
func (s *square) getType() string {
    return "square"
}
```

**Output:**

```
Type: square, Area 4
Type: rectangle, Area 6
```

### **Function as User Defined Type**

Function as user defined type can be declared using the type keyword

```
package main

import "fmt"

func main() {
    areaF := getAreaFunc()
    print(3, 4, areaF)
}

type area func(int, int) int

func print(x, y int, a area) {
    fmt.Printf("Area is: %d\n", a(x, y))
}

func getAreaFunc() area {
    return func(x, y int) int {
        return x * y
    }
}
```

**Output:**

```
12
```

## **Function as values (or Anonymous functions)**

A function in Go is a first-order variable so it can be used as a value as well. It is also called as anonymous functions because a function is not named and can be assigned to a variable and passed around.

They are generally created for short term use or for limited functionality. See the below example. In this example variable **max** assigned a function. Since **max** is created by a function that is not named, it is an anonymous function.

```
package main

import "fmt"

var max = func(a, b int) int {
    if a >= b {
        return a
    }
    return b
}

func main() {
    res := max(2, 3)
    fmt.Println(res)
}
```

**Output:**

```
3
```

# **Special Usages of Function**

## **Function Closures**

Function closures are nothing but anonymous function which can access variables declared outside the function and also retain the current value between different function calls. Let’s see an example. In below case the **count** value is retained between different function calls of **modulus** function

```
package main

import (
    "fmt"
)

func main() {
    modulus := getModulus()
    modulus(-1)
    modulus(2)
    modulus(-5)
}

func getModulus() func(int) int {
    count := 0
    return func(x int) int {
        count = count + 1
        fmt.Printf("modulus function called %d times\n", count)
        if x < 0 {
            x = x * -1
        }
        return x
    }
}
```

**Output:**

```
modulus function called 1 times
modulus function called 2 times
modulus function called 3 times
```

## **Higher Order Function**

Higher-order functions are those functions that either accept a function as a type or return function. Since the function is the first-order variable in Golang they can be passed around and also returned from some function and assigned to a variable. In below example

*   **print** function takes a function of type **func(int, int) int** as an argument

*   **getAreafunc** returns a function of type **func(int, int) int**

```
package main

import "fmt"

func main() {
    areaF := getAreaFunc()
    print(3, 4, areaF)
}

func print(x, y int, area func(int, int) int) {
    fmt.Printf("Area is: %d\n", area(x, y))
}

func getAreaFunc() func(int, int) int {
    return func(x, y int) int {
        return x * y
    }
}
```

**Output:**

```
12
```

## **IIF or Immediately Invoked Function**

**IIF or** Immediately Invoked Function are those function which can be defined and executed at the same time.

```
package main

import "fmt"

func main() {
    squareOf2 := func() int {
        return 2 * 2
    }()
    fmt.Println(squareOf2)
}
```

**Output:**

```
4
```

**Use Cases of IIF functions**

*   When you don't want to expose the logic of the function either within or outside the package. For eg let's say there is a function which is setting some value. You can encapsulate all the logic of setting in an IIF function. This function won't be available for calling either outside or within the package.

## **Variadic Function**

In Go, a function that can accept a dynamic number of arguments is called a Variadic function. Below is the syntax for variadic function. Three dots are used as a prefix before type.

```
func add(numbers ...int)
```

**Code:**

```
package main

import "fmt"

func main() {
    fmt.Println(add(1, 2))
    fmt.Println(add(1, 2, 3))
    fmt.Println(add(1, 2, 3, 4))
}

func add(numbers ...int) int {
    sum := 0
    for _, num := range numbers {
        sum += num
    }
    return sum
}
```

**Output:**

```
3
6
10
```

## **Methods**

Method has a receiver argument. When you attach a function to a type, then that function becomes a method for that type. A receiver can be a struct or any other type. The method will have access to the properties of the receiver and can call the receiver's other methods.

**Function:**

```
func some_func_name(arguments) return_values
```

**Method:**

```
func (receiver receiver_type) some_func_name(arguments) return_values
```

This is the only difference between function and method, but due to it they differ in terms of functionality they offer

*   A function can be used as first-order objects and can be passed around while methods cannot.

*   Methods can be used for chaining on the receiver while function cannot be used for the same.

*   There can exist different methods with the same name with a different receiver, but there cannot exist two different functions with the same name in the same package.

# **Conclusion**

This is all about function in golang. Hope you have liked this article. Please share feedback or mistakes/improvements in comments.

**Next Tutorial** – [Constants](https://golangbyexample.com/constant-golang/)
**Previous Tutorial** –[All basic data types](https://golangbyexample.com/all-basic-data-types-golang/)

*   [complete](https://golangbyexample.com/tag/complete/)*   [function](https://golangbyexample.com/tag/function/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [guide](https://golangbyexample.com/tag/guide/)*