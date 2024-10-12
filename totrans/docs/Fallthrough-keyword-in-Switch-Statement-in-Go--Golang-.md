<!--yml
category: 未分类
date: 2024-10-13 06:21:12
-->

# Fallthrough keyword in Switch Statement in Go (Golang)

> 来源：[https://golangbyexample.com/fallthrough-keyword-golang/](https://golangbyexample.com/fallthrough-keyword-golang/)

**fallthrough** keyword is used in switch statement in golang. This keyword is used in switch case block. If the **fallthrough** keyword is present in the case block, then it will transfer control to the next case even though the current case might have matched.

To more understand this let’s first look at an example without fallthrough keyword

```
package main

import "fmt"

func main() {
    i := 45

    switch {
    case i < 10:
        fmt.Println("i is less than 10")
    case i < 50:
        fmt.Println("i is less than 50")
    case i < 100:
        fmt.Println("i is less than 100")
    }
}
```

**Output:**

```
i is less than 50
```

By default the switch statement matches goes through all the case statement from top to bottom and tries to find the first **case expression** that matches the **switch expression**. Once the matching case is found, it exits  and does not consider the other cases. This is what is happening in above example. Even though i is less than 100 but that case is never executed because the second case is matched and after that it exits

fallthrough keyword allows way around this limitation. See below code for **fallthrough** keyword example. In below example even though the second case matched it went through the third case because of **fallthrough** keyword

```
package main

import "fmt"

func main() {
    i := 45
    switch {
    case i < 10:
        fmt.Println("i is less than 10")
        fallthrough
    case i < 50:
        fmt.Println("i is less than 50")
        fallthrough
    case i < 100:
        fmt.Println("i is less than 100")
    }
}
```

**Output**

```
i is less than 50
i is less than 100
```

**fallthrough** needs to be final statement within the switch block. If it is not then compiler raise error

```
fallthrough statement out of place
```

Below program will raise the above error as we have **fmt.Println** after the **fallthrough** statement

```
package main

import "fmt"

func main() {
    i := 45
    switch {
    case i < 10:
        fmt.Println("i is less than 10")
        fallthrough
    case i < 50:
        fmt.Println("i is less than 50")
        fallthrough
        fmt.Println("Not allowed")
    case i < 100:
        fmt.Println("i is less than 100")
    }
}
```

**Break statement**

Below is the **break** statement example.

```
package main

import "fmt"

func main() {
    switch char := "b"; char {
    case "a":
        fmt.Println("a")
    case "b":
        fmt.Println("b")
        break
        fmt.Println("after b")
    default:
        fmt.Println("No matching character")
    }
}
```

**Output**

```
b
```

**break** statement will terminate the execution of the switch and below line below will never be executed

```
fmt.Println("after b")
```

**Conclusion**

This is all about how fallthrough keyword in used in switch statement in golang

*   [fallthrough](https://golangbyexample.com/tag/fallthrough/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)