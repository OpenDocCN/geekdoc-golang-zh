<!--yml
category: 未分类
date: 2024-10-13 06:18:17
-->

# For Loop in go (golang)

> 来源：[https://golangbyexample.com/for-loop-in-golang/](https://golangbyexample.com/for-loop-in-golang/)

This is the  chapter 10 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** –  [For Range loop](https://golangbyexample.com/understand-for-range-loop-golang/)
**Previous Tutorial** – [Constants](https://golangbyexample.com/constant-golang/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

# **Overview**

When it comes to loop, golang has:

*   for loop

*   for-range loop

In this tutorial, we will be learning about the for loop only. The while loop is missing from go but a while loop can be implemented using a for loop as we will see later in this tutorial.

**for** loop in GO basically has three parts as shown below in the format

*   init part

*   condition part

*   post part

```
for init_part; condition_part; post_part {
   ...
}
```

Here is the sequence of execution of the three parts:

*   The **init part** is executed first before the first iteration

*   The **condition part** is executed before every iteration. If the condition is false the loop will exit otherwise the loop will continue to iterate.

*   The **post part** is executed after every iteration. After this the condition is check, if it is true then the loop is continued otherwise loop exists.

Some points to note about for loop:

*   The parenthesis is not necessary around for loop but the curly braces around the body is necessary.

*   The **init** and **post** part is optional.

*   The **init** part can be any statement with a short declaration, function call or assignment. If the **init** part has the variable declaration, then the scope of that variable is limited to within the for loop.

*   The post part can be any statement but generally contains the increment logic. The post part cannot contain initialization. Compiler will raise error in case we add any initialization logic to the post part

# **Examples**

## **Simple for loop**

```
package main

import "fmt"

func main() {
    for i := 0; i < 5; i++ {
        fmt.Println(i)
    }
}
```

**Output:**

```
0
1
2
3
4
```

## **For loop with only condition**

```
package main

import "fmt"

func main() {
    i := 0
    for i < 5 {
        fmt.Println(i)
        i++
    }
}
```

**Output:**

```
0
1
2
3
4
```

## **For Infinite loop**

```
package main

import (
    "fmt"
    "time"
)

func main() {
    i := 0
    for {
        fmt.Println(i)
        i++
        time.Sleep(time.Second * 1)
    }
}
```

**Output:** Above program prints a infinite loop

```
0
1
2
3
4
5
.
.
```

## **Break Statement in For loop**

**break** statement help exit out of the for loop. None of the statement after break is executed inside the for loop.

```
package main

import (
    "fmt"
)

func main() {
    i := 0
    for {
        fmt.Println(i)
        i++
        if i >= 5 {
            break
        }
    }
}
```

**Output:**

```
0
1
2
3
4
```

## **Continue Statement in For loop**

**continue** statement help skip the current iteration of the for loop. None of the statement after the continue is executed and the execution reaches the start again with next iteration. The use case is when you want to only operate on certain elements of the for loop.

Let's see an exampleIn below program we only want to print non-multiples of 3.

```
package main

import "fmt"

func main() {
    for i := 1; i < 10; i++ {
        if i%3 == 0 {
            continue
        }
        fmt.Println(i)
    }
}
```

**Output:**

```
1
2
4
5
7
8
10
```

## **Nested For Loop**

for loop can also be nested as well. See below example

```
package main

import "fmt"

func main() {
    for i := 0; i < 3; i++ {
        fmt.Printf("Outer loop iteration %d\n", i)
        for j := 0; j < 2; j++ {
            fmt.Printf("i= %d j=%d\n", i, j)
        }
    }
}
```

**Output:**

```
Outer loop iteration 0
i= 0 j=0
i= 0 j=1
Outer loop iteration 1
i= 1 j=0
i= 1 j=1
Outer loop iteration 2
i= 2 j=0
i= 2 j=1
```

## **Function Call and Assignment in Init part**

The below code is an example that we can also have function call or assignment in init part too.

```
package main

import "fmt"

func main() {
    i := 1
    //Function call in the init part in for loop
    for test(); i < 3; i++ {
        fmt.Println(i)
    }

    //Assignment in the init part in for loop
    for i = 2; i < 3; i++ {
        fmt.Println(i)
    }
}
func test() {
    fmt.Println("In test function")
}
```

**Output**

```
In test function
1
2
2
```

## **Implementing while loop using for loop**

Go doesn't have the **while** keyword. Instead it has the **for** keyword only. However **for** keyword can be used to simulate the functionality same as **while**.

**for** loop can be implemented to behave the same as **while** if **initialization_part** and **increment_part** can be skipped. Here is an example:

```
package main

import "fmt"

func main() {
    i := 1
    for i <= 5 {
        fmt.Println(i)
        i++
    }
}
```

**Output:**

```
1
2
3
4
5
```

# **Conclusion**

This is all about for loop in golang. Please share feedback/improvements/mistakes in comments

**Next Tutorial** –  [For Range loop](https://golangbyexample.com/understand-for-range-loop-golang/)
**Previous Tutorial** – [Constants](https://golangbyexample.com/constant-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)