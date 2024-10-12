<!--yml
category: 未分类
date: 2024-10-13 06:07:38
-->

# Understanding uintptr in Golang

> 来源：[https://golangbyexample.com/understanding-uintptr-golang/](https://golangbyexample.com/understanding-uintptr-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Properties](#Properties "Properties")
*   [Purpose](#Purpose "Purpose")*  *# **Overview**

This is an unsigned integer type which is large enough to hold any pointer address. Therefore its size is platform dependent. It is just an integer representation of an address.

# **Properties**

*   A **uintptr** can be converted to **unsafe.Pointer** and viceversa. Later we will talk about where conversion of **uintptr** to **unsafe.Pointer** is useful.

*   Arithmetic can be performed on the **uintptr**. Do note here arithmetic cannot be performed in a **pointer** in Go or **unsafe.Pointer** in Go.

*   **uintptr** even though it holds a pointer address, is just a value and does not reference any object. Therefore
    *   Its value will not be updated if the corresponding object moves. Eg When goroutine stack changes
    *   The corresponding object can be garbage collected. The GC does not consider **uintptr** as live references and hence they can be garbage collected.

# **Purpose**

**uintptr** can be used for below purposes:

*   One purpose of uintptr is to be used along with unsafe.Pointer for unsafe memory access. Arithmetic operations cannot be performed on unsafe.Pointer. To perform such arithmetic
    *   unsafe. Pointer is converted to uintptr
    *   arithmetic is then performed on uintptr
    *   uintptr is converted back to unsafe.Pointer  to access the object now pointed by the address

Be careful that the above steps should be atomic with respect to Garbage Collector, otherwise it could lead to issues. For eg after the first step 1, the referenced object is liable to be collection. If that happens then after step 3, the pointer will be an invalid Go pointer and can crash the program. Look at the unsafe package documentation.

[https://golang.org/pkg/unsafe/#Pointer](https://golang.org/pkg/unsafe/#Pointer)

It lists down when the above conversion can be safe. See the below code for the scenario mentioned above.

In the below code we are doing arithmetic like below to get to address of field **“b”** in struct sample and then printing the value at that address. This below code is atomic with reference to the garbage collector.

```
p := unsafe.Pointer(uintptr(unsafe.Pointer(s)) + unsafe.Offsetof(s.b))
```

```
package main
import (
    "fmt"
    "unsafe"
)
type sample struct {
    a int
    b string
}
func main() {
    s := &sample{a: 1, b: "test"}

   //Getting the address of field b in struct s
    p := unsafe.Pointer(uintptr(unsafe.Pointer(s)) + unsafe.Offsetof(s.b))

    //Typecasting it to a string pointer and printing the value of it
    fmt.Println(*(*string)(p))
}
```

**Output:**

```
test
```

*   Another purpose of uintptr is when you want to save the pointer address value for printing it or storing it. Since the address is just stored and does not reference anything, the corresponding object can be garbage collected.

See below code where we are converting an unsafe.Pointer to uintptr and printing it. Also, note as mentioned before too one the unsafe.Pointer is converted to uinptr, the reference is lost and the reference variable can be garbage collected.

```
package main

import (
    "fmt"
    "unsafe"
)

type sample struct {
    a int
    b string
}

func main() {
    s := &sample{
        a: 1,
        b: "test",
    }
    //Get the address as a uintptr
    startAddress := uintptr(unsafe.Pointer(s))
    fmt.Printf("Start Address of s: %d\n", startAddress)
}
```

**Output:**

The output will be dependent upon the machine as it is an address.

```
Start Address of s: 824634330992
```

*   [data](https://golangbyexample.com/tag/data/)*   [uintptr](https://golangbyexample.com/tag/uintptr/)*