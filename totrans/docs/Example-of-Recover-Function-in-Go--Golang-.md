<!--yml
category: 未分类
date: 2024-10-13 06:26:37
-->

# Example of Recover Function in Go (Golang)

> 来源：[https://golangbyexample.com/recover-example-go/](https://golangbyexample.com/recover-example-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

Go provides a built-in function **recover** for recovering from a panic. Below is the signature of this function

```
func recover() interface{}
```

**defer** function is the only function that is called after the **panic**. So it makes sense to put the **recover** function in the **defer** function only. If the **recover** function is not within the defer function then it will not stop **panic**.

# **Example**

Let’s see an example of recover

```
package main

import "fmt"

func main() {

	a := []string{"a", "b"}
	checkAndPrint(a, 2)
	fmt.Println("Exiting normally")
}

func checkAndPrint(a []string, index int) {
	defer handleOutOfBounds()
	if index > (len(a) - 1) {
		panic("Out of bound access for slice")
	}
	fmt.Println(a[index])
}

func handleOutOfBounds() {
	if r := recover(); r != nil {
		fmt.Println("Recovering from panic:", r)
	}
}
```

**Output**

```
Recovering from panic: Out of bound access for slice
Exiting normally
```

In the above program we have a function **checkAndPrint** which checks and prints slice element at an index passed in the argument. If the index passed is greater than the length of the array then the program panics.  We have added a defer function named **handleOutIfBounds** as well at the start of the function **checkAndPrint**.   This function  contains  the  call to recover function as below

```
if r := recover(); r != nil {
    fmt.Println("Recovering from panic:", r)
}
```

The **recover** function will catch the panic and we can also print the message from the panic. 

```
Recovering from panic: Out of bound access for slice
```

After the recover function the program continues and the control returns to the called function which is  **main** here. That is why we get output as

```
Exiting normally
```

The recover function  returns the value which was passed to the panic function. Therefore it is a good practice to check the return value of the recover function. If the return value is non nil then panic did not happen and recover function was not called with the panic. That is why we have below code in the  defer function **handleOutofBounds**

```
if r := recover(); r != nil 
```

Here if **r** is nil then panic did not happened. So if there is no panic then call to recover will return nil

Note that if the defer function and recover function is not called from the panicking function then it that case also panic can be recovered in the called function as well. Infact it is possible to recover from panic subsequently up in the chain of call stack.

Let’s see an example of this

```
package main

import "fmt"

func main() {
    a := []string{"a", "b"}
    checkAndPrintWithRecover(a, 2)
    fmt.Println("Exiting normally")
}
func checkAndPrintWithRecover(a []string, index int) {
    defer handleOutOfBounds()
    checkAndPrint(a, 2)
}
func checkAndPrint(a []string, index int) {
    if index > (len(a) - 1) {
        panic("Out of bound access for slice")
    }
    fmt.Println(a[index])
}
func handleOutOfBounds() {
    if r := recover(); r != nil {
        fmt.Println("Recovering from panic:", r)
    }
}
```

**Output**

```
Recovering from panic: Out of bound access for slice
Exiting normally
```

Above program is quite same as previous program other than we have a additional function **checkAndPrintWithRecover** which contains the call to 

*   defer function with recover which is **handleOutOfBounds**
*   calls **checkAndPrint** function

**So basically **checkAndPrint** function raises the panic but doesn’t have the recover function instead call to recover lies in the **checkAndPrintWithRecover** function. But still the program is able to recover from panic  as panic can also be recovered in the called function also and subsequently in the chain as well**

***   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)***