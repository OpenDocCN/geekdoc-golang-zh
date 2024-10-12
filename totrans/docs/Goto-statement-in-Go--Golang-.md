<!--yml
category: 未分类
date: 2024-10-13 06:30:53
-->

# Goto statement in Go (Golang)

> 来源：[https://golangbyexample.com/goto-statement-go/](https://golangbyexample.com/goto-statement-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

Goto statement allows unconditional jump to a labeled statement with in the same function . Below is the format of the goto statement

```
goto label
...
...
label: statement
```

The label can be valid go statement other than a keyword. As soon as the goto statement is encountered the control is transferred to the referred label and execution continues from there. The label is only visible inside the function where it has been declared. Any reference from outside the function will result in a compilation error.

# **Example**

Let’s see a simple example of goto statement

```
package main
import "fmt"
func main() {
    learnGoTo()
}
func learnGoTo() {
    fmt.Println("a")
    goto FINISH
    fmt.Println("b")
FINISH:
    fmt.Println("c")
}
```

**Output**

```
a
c
```

In the above example we have a **goto** statement as

```
goto FINISH
```

The **FINISH** label is as below

```
FINISH:
    fmt.Println("c")
```

As soon as the program encounters the **goto** statement it jumps to the referred label. That is why below line never gets executed and **b** is never printed.

```
fmt.Println("b")
```

Both label and goto need to belong to same function otherwise it will raise compilation error. This is because scope of label is within the function in which it is declared as mentioned above. For example below program raises a compilation error

```
./main.go:11:7: label FINISH not defined
./main.go:17:1: label FINISH defined and not used
```

```
package main

import "fmt"

func main() {
	learnGoTo()
}

func learnGoTo() {
	fmt.Println("a")
	goto FINISH
	fmt.Println("b")

}

func test() {
FINISH:
	fmt.Println("c")
}
```

The label can be before goto statement as well. See below example. The program can be use to print all odd numbers before 10\. Notice that label  **START** lies before goto here

```
package main
import "fmt"
func main() {
    learnGoTo()
}
func learnGoTo() {
    i := 0
    fmt.Println("here")
START:
    for i < 10 {
        if i%2 == 0 {
            i++
            goto START
        }  
        fmt.Println(i)
        i++
    }
}
```

Use of Goto is not recommended practice as readability is bad and is generally a source of lot of bugs too.  Any thing which is achieved using goto can be achieved using other go constructs

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*