<!--yml
category: 未分类
date: 2024-10-13 06:18:50
-->

# Copy an array or slice in Go (Golang)

> 来源：[https://golangbyexample.com/copy-an-array-or-slice-golang/](https://golangbyexample.com/copy-an-array-or-slice-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Copy an array](#Copy_an_array "Copy an array")
*   [Copy a slice](#Copy_a_slice "Copy a slice")*  *# **Overview**

Array are value types in go while slice is a reference type. Hence there is a difference in the way how an array or slice can be copied to another array or slice respectively.

# **Copy an array**

As mentioned above, an array is value types in go. So an array variable name is not a pointer to the first element, in fact, it denotes the entire array. A copy of the array will be automatically created when

*   An array variable is assigned to another array variable.

*   An array variable is passed as an argument to a function.

Let’s see above point with an example

```
package main

import "fmt"

func main() {
	sample1 := [2]string{"a", "b"}
	fmt.Printf("Sample1 Before: %v\n", sample1)
	sample2 := sample1
	sample2[1] = "c"
	fmt.Printf("Sample1 After assignment: %v\n", sample1)
	fmt.Printf("Sample2: %v\n", sample2)
	test(sample1)
	fmt.Printf("Sample1 After Test Function Call: %v\n", sample1)
}
func test(sample [2]string) {
	sample[0] = "d"
	fmt.Printf("Sample in Test function: %v\n", sample)
}
```

**Output**

```
Sample1 Before: [a b]
Sample1 After assignment: [a b]
Sample2: [a c]
Sample in Test function: [d b]
Sample1 After Test Function Call: [a b]
```

In above example,

*   we assigned the **sample1** to **sample2** and we then changed 0th index at **sample2** to have a different value.  After that when we print **sample1**, we see that it hasn’t changed. This is because when we assign **sample1** to **sample2**, a copy is created and changing **sample2** doesn’t have any effect on **sample1**

*   We passed **sample1** to the test function and then again changed its value in the test function at 0th index.  After that when we print **sample1**, we see that it hasn’t changed. The reason is the same, when **sample1** is passed as an argument to test function a copy of **sample1** is created.

# **Copy a slice**

go **builtin** package provides a **copy** function that can be used to copy a slice. Below is the signature of this function. It returns the number of elements copied.

```
func copy(dst, src []Type) int
```

There are two cases to be considered while using the copy function:

*   If the length of **src** is greater than length of **dst,** then number of elements copied is length of **dst**

*   If the length of **dst** is greater than length of **src,** then number of elements copied is length of **src**

Basically the number of elements copied is minimum of length of **(src, dst). **

Also to note that once the copy is done then any change in **dst** will not reflect in **src** and vice versa

```
package main

import "fmt"

func main() {
    src := []int{1, 2, 3, 4, 5}
    dst := make([]int, 5)

    numberOfElementsCopied := copy(dst, src)
    fmt.Printf("Number Of Elements Copied: %d\n", numberOfElementsCopied)
    fmt.Printf("dst: %v\n", dst)
    fmt.Printf("src: %v\n", src)

    //After changing numbers2
    dst[0] = 10
    fmt.Println("\nAfter changing dst")
    fmt.Printf("dst: %v\n", dst)
    fmt.Printf("src: %v\n", src)
}
```

**Output**

```
Number Of Elements Copied: 5
dst: [1 2 3 4 5]
src: [1 2 3 4 5]

After changing dst
dst: [10 2 3 4 5]
src: [1 2 3 4 5]
```*