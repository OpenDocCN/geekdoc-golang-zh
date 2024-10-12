<!--yml
category: 未分类
date: 2024-10-13 06:26:17
-->

# Recover function return value in Go (Golang)

> 来源：[https://golangbyexample.com/recover-return-value-golang/](https://golangbyexample.com/recover-return-value-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

The recover function  returns the value which was passed to the panic function. Therefore it is a good practice to check the return value of the recover function. If the return value is non nil then panic did not happen and recover function was not called with the panic.

# **Example**

Let’s see a program to fully understand it

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

In the above program we have a function **checkAndPrint** which checks and prints slice element at an index passed in the argument. If the index passed is greater than the length of the array then the program panics.  We have added a defer function named **handleOutIfBounds** as well at the start of the function **checkAndPrint**.   This function  contains  the  call to recover function  as below

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

The recover function returns the value which was passed to the panic function. That is why we have below code in the  defer function **handleOutofBounds**

```
if r := recover(); r != nil 
```

Here if **r** is nil then panic did not happened. So if there is no **panic** then call to **recover** will return nil

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*