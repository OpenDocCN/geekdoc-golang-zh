<!--yml
category: 未分类
date: 2024-10-13 06:11:02
-->

# Return a function from a function in Go (Golang)

> 来源：[https://golangbyexample.com/return-func-from-func-go/](https://golangbyexample.com/return-func-from-func-go/)

In Golang function are first-order variables meaning that

*   They can be assigned to a variable

*   Passed around as function argument

*   Returned from a function

While returning a function from another function, the exact signature of the function has to be specified return list. As in below example

*   The return type of **getAreaFunc** function is **func(int, int) int**

```
func getAreaFunc() func(int, int)
```

*   **getAreaFunc** function hence it can return a function of type **func(int, int) int**

**Code:**

```
package main

import "fmt"

func main() {
    areaF := getAreaFunc()
    res := areaF(2, 4)
    fmt.Println(res)
}

func getAreaFunc() func(int, int) int {
    return func(x, y int) int {
        return x * y
    }
}
```

**Output:**

```
8
```