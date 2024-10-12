<!--yml
category: 未分类
date: 2024-10-13 06:18:12
-->

# Switch Statement in Go (Golang)

> 来源：[https://golangbyexample.com/switch-statement-golang/](https://golangbyexample.com/switch-statement-golang/)

This is the  chapter 13 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [Defer keyword](https://golangbyexample.com/defer-golang)
**Previous Tutorial** – [If Else](https://golangbyexample.com/understand-if-else-statement-golang/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

# **Overview**

Switch statement are a perfect way to prevent a if-else ladder. Here is the format for switch statement

```
switch statement; expression {
case expression1:
     //Dosomething
case expression2:
     //Dosomething
default:
     //Dosomething
}
```

This is how switch works. Give a **switch expression**, it goes through all cases and tries to find the first **case expression** that matches the **switch expression** otherwise the default case is executed if present. The order of matching is from top to bottom and then left to right (when the case contains multiple expressions as we will see later in this tutorial).

# **Important Points**

Some important things to know about switch before we move to code examples

*   Both **switch statement** and **switch expression** are optional statement. So four possible scenario exist for them

Both **switch statement** and **switch expression** are present

```
switch statement; expression {
   ... 
}
```

Only **switch statement** is present. Notice the syntax below. Semicolon needs to be present after **switch statement**

```
switch statement; {
    ...
}
```

Only **switch expression** is present. Notice the syntax below. No semicolon after **switch expression**.

```
switch expression {
   ..
}
```

Both **switch statement** and **switch expression** are absent.

```
switch {
  ... 
}
```

*   If the **switch expression** is not provided then the default type assumed by the compiler is **boolean**.

*   The type of **switch expression** and all of **case expression*** should match other wise there will be a compiler error raised. When **switch expression** is not provided the type of all **case expression*** needs to be a boolean too.

*   **switch statement** can be any statement with a short declaration, function call or assignment. If **switch statement** has a variable declaration then the scope of that variable is limited to the switch block

*   It can have any number of case statement

*   default case is optional

*   case can have multiple expression separated by comma. Then case will look like as below

```
case expression1_1, expression_2 ...:
```

*   case block also allows **fallthrough** keyword which transfer control to the next case even though the current case might have matched

*   break statement that is needed in switch cases in many other languages is not needed in go. Go automatically provides a break statement at the end of each case block. However, it is also correct to explicitly use the **break** keyword which terminates execution out of switch statement.

*   Two cases cannot have the same constant value. There will be a compiler error raised in that case.

*   switch statement can also be used as a type switch, where it is used to know the type of an empty interface at run time as we will see in examples below

# **Examples**

Let’s see some simple example which illustrates the points above.

## **Both switch statement and switch expression**

```
package main

import "fmt"

func main() {
    switch ch := "b"; ch {
    case "a":
        fmt.Println("a")
    case "b", "c":
        fmt.Println("b or c")    
    default:
        fmt.Println("No matching character")    
    }

    //fmt.Println(ch)

} 
```

**Output:**

```
b or c
```

Some points to note:

*   In the above example, we have switch statement having a short declaration.
*   Then the switch case matches the switch expression which is “b” here.
*   Also, there can be multiple expression in the case, as we see above that second case has two expressions

```
 case "b", "c":
```

*   **ch** variable is only available within **switch** block. Uncomment the line **fmt.Println(ch)** which is present outside switch block and it will raise an error

```
undefined: ch
```

## **Both switch statement and switch expression absent**

Let’s see another example where we omit the **switch statement** as well as the **switch expression**

```
package main

import "fmt"

func main() {
    age := 45
    switch {
    case age < 18:
        fmt.Println("Kid")
    case age >= 18 && age < 40:
        fmt.Println("Young")
    default:
        fmt.Println("Old")
    }
}
```

**Output**

```
Old
```

Couple of points to notice about above example:

*   Since we omitted the switch expression, the default type of switch expression is boolean. Each of the case expressions also evaluates to boolean hence the program works fine.
*   Default case is executed in the above example as none of the case expression matched.

## **Only switch statement**

Notice the **';'** after the statement

```
package main

import "fmt"

func main() {

    switch age := 29; {
    case age < 18:
        fmt.Println("Kid")
    case age >= 18 && age < 40:
        fmt.Println("Young")
    default:
        fmt.Println("Old")
    }
}
```

**Output:**

```
Young
```

## **Only switch expression**

```
package main

import "fmt"

func main() {
    char := "a"
    switch char {
    case "a":
        fmt.Println("a")
    case "b":
        fmt.Println("b")
    default:
        fmt.Println("No matching character")
    }
}
```

**Output**

```
a
```

## **Duplicate case**

Two case statement cannot have the same constant. For example in below case there will be a compiler error raised as  "a"  is present in two case

```
duplicate case "a" in switch
```

```
package main

import "fmt"

func main() {
    switch "a" {
    case "a":
        fmt.Println("a")
    case "a":
        fmt.Println("Another a")
    case "b":
        fmt.Println("b")
    default:
        fmt.Println("No matching character")
    }
}
```

## **Fallthrough keyword**

See below code for **fallthrough** keyword example. In below example even though the second case matched it went through the third case because of **fallthrough** keyword

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

## **Break statement**

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

## **Type Switch**

Switch statement can also be used to know the type of an interface at run time as shown in below example. The type switch compare types instead of values

```
package main

import "fmt"

func main() {
    printType("test_string")
    printType(2)
}

func printType(t interface{}) {
    switch v := t.(type) {
    case string:
        fmt.Println("Type: string")
    case int:
        fmt.Println("Type: int")
    default:
        fmt.Printf("Unknown Type %T", v)
    }
}
```

**Output:**

```
Type: string
Type: int
```

# **Conclusion**

This is all about switch statement in go. Hope you have liked the article. Please share feedback/improvements/mistakes in comments.

**Next Tutorial** – [Defer keyword](https://golangbyexample.com/defer-golang)
**Previous Tutorial** – [If Else](https://golangbyexample.com/understand-if-else-statement-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [switch](https://golangbyexample.com/tag/switch/)