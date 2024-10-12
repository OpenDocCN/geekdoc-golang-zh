<!--yml
category: 未分类
date: 2024-10-13 06:28:55
-->

# * or dereferencing pointer in Go (Golang)

> 来源：[https://golangbyexample.com/dereferencing-pointer-golang/](https://golangbyexample.com/dereferencing-pointer-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

***** operator can be used to:

*   Dereference a pointer which means getting the value at the address stored in the pointer.

*   Change the value at that pointer location as well

# **Program**

```
package main

import "fmt"

func main() {
	a := 2
	b := &a
	fmt.Println(a)
	fmt.Println(*b)

	*b = 3
	fmt.Println(a)
	fmt.Println(*b)

	a = 4
	fmt.Println(a)
	fmt.Println(*b)
}
```

**Output**

```
2
2
3
3
4
4
```

Both **a** and ***b** refer to the same variable internally.  Hence the changing the value of one reflects in another. Also, ***** and **&** can be used together as well. But they will cancel out each other.

Hence below two are equivalent and will print 2

*   a
*   *&a

Also below three are equivalent and will print the address of variable a which is stored in b variable

*   b
*   *&b
*   &*b

Note:  *a is not a valid operation as a is not a pointer

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*