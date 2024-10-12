<!--yml
category: 未分类
date: 2024-10-13 06:18:07
-->

# Understand If Else Statement in Go (Golang)

> 来源：[https://golangbyexample.com/understand-if-else-statement-golang/](https://golangbyexample.com/understand-if-else-statement-golang/)

This is the  chapter 12 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [Switch](https://golangbyexample.com/switch-statement-golang/)
**Previous Tutorial** – [For Range loop](https://golangbyexample.com/understand-for-range-loop-golang/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

# **Overview**

Go has if-else statement similar to any other programming language to perform the basic conditional logic. Below is the format for if-else statement in golang

```
if condition {
   //Do something
} else if condition {
   //Do something
} else {
   //Do something
```

Before we move further let’s talk about the **condition** first. Only a statement or a combination of statements that result in a boolean are allowed for a condition in if. false boolean is treated as false in a condition in go and true boolean is treated as true. As mentioned above, the condition can be composed of multiple statements combined by operators in Go such as &&, ||, >, <, >=, <=, ! etc.

Now let’s look into the if-else statement in detail to understand the small things. Go supports below formats for the if-else statement

*   Only if
*   If Else
*   If Else Ladder
*   Nested if-else
*   If with a short statement

## **If statement**

if statement alone has below format

```
if condition {
   //Do something
}
```

If the condition is true then the statement inside the braces is executed.  Some points to note about if statement

*   Brackets can be omitted around the condition.

*   Opening and closing braces after condition are mandatory

Let’s see a working example. Below program checks if a number is greater than 5.

```
package main

import "fmt"

func main() {
    a := 6
    if a > 5 {
        fmt.Println("a is greater than 5")
    }
}
```

**Output**

```
a is greater than 5
```

Let’s see another example of multiple statement in a if condition. Below is a program to check if a number lies in a particular range. Notice that multiple statement in the condition are joined by the && operator.

```
package main

import "fmt"

func main() {
    a := 4
    if a > 3 && a < 6 {
        fmt.Println("a is within range")
    }
}
```

**Output**

```
a is within range
```

## **If Else Statement**

If Else statement has below format

```
if condition {
   //Do something
} else {
   //Do something
}
```

If the condition is true then the statement inside the if block is executed otherwise the statement inside the else block is executed. Some points to note about if-else statement.

*   The else keyword should be on the same line as the closing brace for it. If not there will be below compiler error.

```
syntax error: unexpected else, expecting }
```

Let's see a small example of if else statement. In below program we use if else statement to figure out the max number of 2 numbers

```
package main

import "fmt"

func main() {
    a := 1
    b := 2

    if a > b {
        fmt.Println("a is greater than b")
    } else {
        fmt.Println("b is greater than a")
    }
}
```

**Output**

```
b is greater than a
```

## **If Else Ladder**

If Else ladder has the below format

```
if condition1 {
   //Do something
} else if condition2 {
   //Do something
} else {
  //Do something
}
```

Some points to note about this if else ladder

*   Any number of else if statement can be added in the middle

*   else if should lie on the same line as the previous closing brace

Below is a working code example. The code given an age is using a if else ladder to find out weather a person is "Kid", "Young" or "Old".

```
package main

import "fmt"

func main() {
    age := 29
    if age < 18 {
        fmt.Println("Kid")
    } else if age >= 18 && age < 40 {
        fmt.Println("Young")
    } else {
        fmt.Println("Old")
    }
}
```

**Output:**

```
Young
```

## **Nested If Else**

Below are some one of the possible format for nested if else.

Only nested if

```
if condition {
  //Do something
  if condition2 { 
    //Do something
  }
  //Do something
}
```

Nested if else

```
if condition1 {
   //....
   if condition2 {
      //...
   } else {
      //...
   }
  //...
}
```

Below combination is also possible for nested if else

```
if condition1 {
   //...
} else {
   //...
   if condition2 {
      //...
   } else {
     //....
   }
   //....
}
```

Let's see a working example of nested if else. In below program we print the max of three numbers using nested if else.

```
package main

import "fmt"

func main() {
    a := 1
    b := 2
    c := 3
    if a > b {
        if a > c {
            fmt.Println("Biggest is a")
        } else if b > c {
            fmt.Println("Biggest is b")
        }
    } else if b > c {
        fmt.Println("Biggest is b")
    } else {
        fmt.Println("Biggest is c")
    }
}
```

**Output:**

```
Biggest is c
```

## **If with short statement**

If statement also supports a statement before the condition. This statement will be executed before the condition. There can also be new initialized variable in the statement. Below is the format for that.

```
if statement; condition {
   //Do something
}
```

The initialization if present in the statement will be a short declaration. Notice that var keyword is not supported in the statement. Let's see a working example

```
package main

import "fmt"

func main() {
    if a := 6; a > 5 {
        fmt.Println("a is greater than 5")
    }
}
```

**Output**

```
a is greater than 5
```

The variable that is initialized in if statement is available inside all the branches. As in below example variable **a** is also available in the else block.

```
package main

import "fmt"

func main() {
    if a := 1; a > 5 {
        fmt.Println("a is greater than 5")
    } else {
        fmt.Println("a is less than 5")
    }
}
```

**Output:**

```
a is less than 5
```

# **If Conditions**

We mentioned at the start that only boolean values or statement that result in boolean value are allowed in the if condition. Let's see a working code of the error that comes in case of using any else than boolean

```
package main

import "fmt"

func main() {
    if 1 {
        fmt.Println("a is greater than 5")
    }
}
```

**Output:** Below compiler error is raised

```
non-bool 1 (type int) used as if condition
```

# **Ternary Operator**

There is no ternary operator in Go, hence you need to use if else statements in place of that.

# **Conclusion**

That is all about if else statement in go. Hope you have liked this article. Please share feedback/improvements/mistakes in comments

**Next Tutorial** – [Switch](https://golangbyexample.com/switch-statement-golang/)
**Previous Tutorial** – [For Range loop](https://golangbyexample.com/understand-for-range-loop-golang/)

*   [complete](https://golangbyexample.com/tag/complete/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [guide](https://golangbyexample.com/tag/guide/)*   [understand](https://golangbyexample.com/tag/understand/)*   [using](https://golangbyexample.com/tag/using/)