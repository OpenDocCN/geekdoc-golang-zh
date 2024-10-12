<!--yml
category: 未分类
date: 2024-10-13 06:18:28
-->

# Understanding Array in Go (Golang) – Complete Guide

> 来源：[https://golangbyexample.com/understanding-array-golang-complete-guide/](https://golangbyexample.com/understanding-array-golang-complete-guide/)

This is the  chapter 17 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [Slice](https://golangbyexample.com/slice-in-golang/)
**Previous Tutorial** – [Struct](https://golangbyexample.com/struct-in-golang-complete-guide/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

Table of Contents

 **   [Overview](#Overview "Overview")
    *   [Definition](#Definition "Definition")
*   [Declaration of an array](#Declaration_of_an_array "Declaration of an array")
*   [Accessing array elements](#Accessing_array_elements "Accessing array elements")
*   [Arrays are value in go](#Arrays_are_value_in_go "Arrays are value in go")
*   [Different ways of iterating an array](#Different_ways_of_iterating_an_array "Different ways of iterating an array")
*   [MultiDimensional Arrays](#MultiDimensional_Arrays "MultiDimensional Arrays")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

Similar to any other programming language, golang also has **array** data structure. But in go, **arrays** behave little differently than other languages and also we have something called slice in golang which is like a reference to an array. In this article, we will study only array.

## **Definition**

An array is a contiguous collection of elements of the same type. It is an ordered sequence of elements stored contiguously in memory

Here is the format for the declaration of an array

```
sample := [size_of_array]{type}{a1, a2... an}
```

*   size_of_array – number of elements in the array

*   <type> is type of each element in the array

*   a1, a2 … an are the actual elements.

In golang, the size of the array is part of its type. So  This means that two arrays that have a different number of elements are of two different types and one cannot be assigned to another. Below error will be raised in case we try to assign two arrays of different length

```
cannot use sample1 (type [1]int) as type [2]int in assignment
```

The code is:

```
sample1 := [1]int{1}
sample2 := [2]int{1,2}

sample2 = sample1
```

For the same reason the length of array is fixed during create and cannot be changed later.

# **Declaration of an array**

Both number of elements and actual elements are optional in the array declaration.

In below example, we see 4 ways of declaring of an array

*   Specifying both the length of the array and actual elements. Eg.

```
[2]int{1, 2}
```

*   Only length – In this case all the actual elements are filled up with default value zero of that type. Eg

```
[2]int{}
```

*   Only actual elements – In this case, the length of array will be equal to the number of actual elements. The symbol **‘…’** needs to be used within square brackets like this **[…]** when not specifying the length. The symbol is an instruction to the compiler to calculate the length.

```
[...]int{2, 3}
```

*   Without length and actual elements – an empty array will be created in this case. Similar to above the symbol **‘…’** also needs to be used in this case as well.

```
[...]int{}
```

Let’s see a code example illustrating above points. Also please keep in mind that the builtin function **len()** can be used to calculate the length of an array. In below program we are using **len()** function to calculate the length of the array.

```
package main

import "fmt"

func main() {
    //Both number of elements and actual elements
    sample1 := [2]int{1, 2}
    fmt.Printf("Sample1: Len: %d, %v\n", len(sample1), sample1)

    //Only actual elements
    sample2 := [...]int{2, 3}
    fmt.Printf("Sample2: Len: %d, %v\n", len(sample2), sample2)

    //Only number of elements
    sample3 := [2]int{}
    fmt.Printf("Sample3: Len: %d, %v\n", len(sample3), sample3)

    //Without both number of elements and actual elements
    sample4 := [...]int{}
    fmt.Printf("Sample4: Len: %d, %v\n", len(sample4), sample4)
}
```

**Output**

```
Sample1: Len: 2, [1 2]
Sample2: Len: 2, [2 3]
Sample3: Len: 2, [0 0]
Sample4: Len: 0, []
```

Notice in the above example that for **sample3** variable the actual elements are filled up with the default value of int which is 0.

It is also ok if the actual elements specified are less than the length of the array. The rest of the elements are filled up with the default value of the type specified. See the below example. The length of the array specified is 4 while only 2 actual elements are declared. Hence the remaining two elements are assigned value 0 which is the default zero value of an **int**.

```
package main

import "fmt"

func main() {
    sample := [4]int{5, 8}
    fmt.Printf("Sample: Len: %d, %v\n", len(sample), sample)
}
```

**Output**

```
Sample: Len: 4, [5 8 0 0]
```

# **Accessing array elements**

Since array element are stored in contiguous manner, we can access an array element using an index. Similarly individual array elements can also be assigned a value using index. Accessing out of bound index will cause a compilation error. See below examples illustrating these points. The first index position will be **zero** and last will **(length_of_array-1)**

```
package main

import "fmt"

func main() {
    sample := [2]string{"aa", "bb"}

    fmt.Println(sample[0])
    fmt.Println(sample[1])

    sample[0] = "xx"
    fmt.Println(sample)
    //sample[3] = "yy"
}
```

**Output**

```
aa
bb
[xx bb]
```

On uncommenting the below line

```
sample[3] = "yy"
```

, it will give compilation error

```
invalid array index 3 (out of bounds for 2-element array)
```

# **Arrays are value in go**

Array are value type in go. So an array variable name is not a pointer to the first element in fact it denotes the entire array. A copy of the array will be created when

*   An array variable is assigned to another array variable.
*   An array variable is passed as an argument to a function.

Let’s see above point with another example

```
package main

import "fmt"

func main() {
    sample1 := [2]string{"a", "b"}
    fmt.Printf("Sample1 Before: %v\n", sample1)
    sample2 := sample1
    sample2[0] = "c"
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
Sample2: 
Sample in Test function: [d b]
Sample1 After Test Function Call: [a b]
```

In above example,

*   we assigned the **sample1** to **sample2** and we then changed 0th index at **sample2** to have a different value.  After that when we print **sample1**, we see that it hasn’t changed. This is because when we assign **sample1** to **sample2**, a copy is created and changing **sample2** doesn’t have any effect on **sample1**

*   We passed **sample1** to the test function and then again changed its value in the test function at 0th index.  After that when we print **sample1**, we see that it hasn’t changed. The reason is same, when **sample1** is passed as an argument to test function a copy of **sample1** is created.

# **Different ways of iterating an array**

An array can be iterated using:

*   Using for loop

*   Using for-range loop

Let’s see a code example for both

```
package main

import "fmt"

func main() {
    letters := [3]string{"a", "b", "c"}
    //Using for loop
    fmt.Println("Using for loop")
    len := len(letters)
    for i := 0; i < len; i++ {
        fmt.Println(letters[i])
    }
    //Using for-range operator
    fmt.Println("\nUsing for-range loop")
    for i, letter := range letters {
        fmt.Printf("%d %s\n", i, letter)
    }
}
```

**Output**

```
Using for loop
a
b
c

Using for-range loop
0 a
1 b
2 c
```

# **MultiDimensional Arrays**

Below is the format for declaring a two dimensional array

```
sample := [x][y]{type}{{a11, a12 .. a1y},
                       {a21, a22 .. a2y},
                       {.. },
                       {ax1, ax2 .. axy}} 
```

where

*   x denotes the number of rows
*   y denotes the number of columns
*   aij denotes an element present at i row and j column

The same idea can be extended to three dimensions, four dimensions, and so on. All the rules we discussed above also apply to multidimensional arrays too.

Let's see a code example

```
package main

import "fmt"

func main() {
    sample := [2][3]int{{1, 2, 3}, {4, 5, 6}}
    fmt.Println("First Run")
    for _, row := range sample {
        for _, val := range row {
            fmt.Println(val)
        }
    }

    sample[0][0] = 6
    sample[1][2] = 1
    fmt.Println("\nSecond Run")
    for _, row := range sample {
        for _, val := range row {
            fmt.Println(val)
        }
    }
}
```

**Output**

```
First Run
1
2
3
4
5
6

Second Run
6
2
3
4
5
1
```

In above example we access the element of two dimensional array using index for both first and second dimension

```
sample[0][0] = 6
```

Also notice how we are traversing the two dimensional array. We need to use nested range . The first range traverses the arrays of array. The second range traverses the individual array after that.

# **Conclusion**

This is all about array in Golang. Hope you have liked this article. Please share feedback/improvements/mistakes in comments.

**Next Tutorial** – [Slice](https://golangbyexample.com/slice-in-golang/)
**Previous Tutorial** – [Struct](https://golangbyexample.com/struct-in-golang-complete-guide/)

*   [array](https://golangbyexample.com/tag/array/)*   [complete guide](https://golangbyexample.com/tag/complete-guide/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*