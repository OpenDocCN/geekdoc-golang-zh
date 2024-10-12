<!--yml
category: 未分类
date: 2024-10-13 06:10:53
-->

# Function Closures in Go (Golang)

> 来源：[https://golangbyexample.com/function-closures-golang/](https://golangbyexample.com/function-closures-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")
    *   [Example 1](#Example_1 "Example 1")
    *   [Example 2:](#Example_2 "Example 2:")
    *   [Example 3:](#Example_3 "Example 3:")*  *# **Overview**

Function closures are nothing but an anonymous function that can access variables declared outside the function and also retain the current value of those variables between different function calls. Anonymous functions are functions that are not named.

A closure happens when a function is defined within a different function and the inner function can access the variable of the outer function.

You can read more about closures here

[https://en.wikipedia.org/wiki/Closure_(computer_programming)](https://en.wikipedia.org/wiki/Closure_(computer_programming))

# **Code:**

Let’s see an example. Three things to notice in Example 1

*   The getModulus function returns a closure. It is assigned to a variable **modulus**

*   This closure function can access the **count** variable defined outside its body.

*   The value of the **count** variable is retained between different function calls of modulus function

## **Example 1**

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

## **Example 2:**

Below is also another example of a closure function. The function is able to access the **valueOutside** variable.

```
package main

import "fmt"

func main() {
    valueOutside := "somevalue"
    func() {
        fmt.Println(valueOutside)
    }()
}
```

**Output:**

```
somevalue
```

## **Example 3:**

In the below example, the closure function is able to access the count variable, as well as the value of the count variable, which is retained between different function calls.

```
package main

import "fmt"

func main() {
    count := 0
    for i := 1; i <= 5; i++ {
        func() {
            count++
            fmt.Println(count)
        }()
    }
}
```

**Output**

```
1
2
3
4
5
```*