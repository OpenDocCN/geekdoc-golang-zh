<!--yml
category: 未分类
date: 2024-10-13 06:20:36
-->

# Declare/Initialize/Create an array or slice in Go (Golang)

> 来源：[https://golangbyexample.com/declare-initialize-create-array-slice-golang/](https://golangbyexample.com/declare-initialize-create-array-slice-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Creating a slice](#Creating_a_slice "Creating a slice")
    *   [Using the []<type>{} format](#Using_the_format "Using the []<type>{} format")
    *   [Creating a slice from another slice or array](#Creating_a_slice_from_another_slice_or_array "Creating a slice from another slice or array")
        *   [Create a slice from Array](#Create_a_slice_from_Array "Create a slice from Array")
        *   [Create a slice from slice](#Create_a_slice_from_slice "Create a slice from slice")
    *   [Using the make function](#Using_the_make_function "Using the make function")
    *   [Using the new function](#Using_the_new_function "Using the new function")
*   [Create an array](#Create_an_array "Create an array")*  *# **Overview**

Similar to any other programming language, golang also has an array data structure. But in go, arrays behave little differently than other languages and also we have something called slice in golang which is like a reference to an array. Slice is more powerful and convenient to use than an array.  Slice, in fact, is more analogous to arrays in another programming language.

In this article, we will learn how to

*   Create a slice instance

*   Create an array instance

# **Creating a slice**

There are four ways of creating a slice

*   Using the []<type>{} format
*   Creating a slice from another slice or array
*   Using make
*   Using new

Let’s look at each of above method one by one.

## **Using the []<type>{} format**

The most common way of declaring a slice is this

```
s := []int
```

It declares an empty of slice of 0 length and 0 capacity. We can also initialise the slice during declaration

```
s := []int{1,2}
```

It declares a slice of integers of length 2 and also the capacity of 2\. The capacity will be equal to the actual slice elements specified. We also have two library functions provided by go which can be used to know the **length** and **capacity** of a slice.

*   **len()** function – for  length of the slice

*   **cap()** function – for capacity of the slice

Let’s see a small program which shows the above points

```
package main

import "fmt"

func main() {
    sample := []int{}
    fmt.Println(len(sample))
    fmt.Println(cap(sample))
    fmt.Println(sample)

    letters := []string{"a", "b", "c"}
    fmt.Println(len(letters))
    fmt.Println(cap(letters))
    fmt.Println(letters)
}
```

**Output**

```
0
0
[]

3
3
[a b c]
```

When the actual elements are not specified, then both length and capacity of the slice is zero. When actual elements are specified , both  length and capacity is equal to the number of actual elements specified.

## **Creating a slice from another slice or array**

A slice can be created by re-slicing an exiting slice or array.

### **Create a slice from Array**

The format for creating a new slice by re-slicing an existing array is

```
[n]sample[start:end]
```

The above operation will return a new slice from the arraystarting from index **start** to index **end-1\.** So the element at index **end** is not included in the newly created slice. While re-slicing, both start, and the end index are optional.

*   The default value of the start index is zero

*   The default value of the end index is the length of the array

Let’s see an example.

```
package main

import "fmt"

func main() {
    numbers := [5]int{1, 2, 3, 4, 5}

    //Both start and end
    num1 := numbers[2:4]
    fmt.Println("Both start and end")
    fmt.Printf("num1=%v\n", num1)
    fmt.Printf("length=%d\n", len(num1))
    fmt.Printf("capacity=%d\n", cap(num1))

    //Only start
    num2 := numbers[2:]
    fmt.Println("\nOnly start")
    fmt.Printf("num1=%v\n", num2)
    fmt.Printf("length=%d\n", len(num2))
    fmt.Printf("capacity=%d\n", cap(num2))

    //Only end
    num3 := numbers[:3]
    fmt.Println("\nOnly end")
    fmt.Printf("num1=%v\n", num3)
    fmt.Printf("length=%d\n", len(num3))
    fmt.Printf("capacity=%d\n", cap(num3))

    //None
    num4 := numbers[:]
    fmt.Println("\nOnly end")
    fmt.Printf("num1=%v\n", num4)
    fmt.Printf("length=%d\n", len(num4))
    fmt.Printf("capacity=%d\n", cap(num4))
}
```

**Output**

```
Both start and end
num1=[3 4]
length=2
capacity=3

Only start
num1=[3 4 5]
length=3
capacity=3

Only end
num1=[1 2 3]
length=3
capacity=5

Only end
num1=[1 2 3 4 5]
length=5
capacity=5
```

Notice in above example that

*   length of newly created slice = (**end**–**start**)

*   capacity of newly created slice = (**length_of_array**–**start**)

The **num1** slice would look like

![](img/33586fc71a4b7b6f0294dc7dc24f41df.png)

The newly created slices still refer the original array. To check this change element at any one of the index of the array and then reprint the slice

```
numbers[3] = 8
fmt.Printf("num1=%v\n", num2)
fmt.Printf("num3=%v\n", num3)
fmt.Printf("num4=%v\n", num4)
```

Here is the output:

```
num1=[3 8 5]
num3=[1 2 3 8]
num4=[1 2 3 8 5]
```

This proves that each of the new slice is still referring to the original array.

### **Create a slice from slice**

Whatever we discussed about re-slicing from an array also applies here as well. See below example which illustrates same thin

```
package main

import "fmt"

func main() {
    numbers := []int{1, 2, 3, 4, 5}

    //Both start and end
    num1 := numbers[2:4]
    fmt.Println("Both start and end")
    fmt.Printf("num1=%v\n", num1)
    fmt.Printf("length=%d\n", len(num1))
    fmt.Printf("capacity=%d\n", cap(num1))

    //Only start
    num2 := numbers[2:]
    fmt.Println("\nOnly start")
    fmt.Printf("num1=%v\n", num2)
    fmt.Printf("length=%d\n", len(num2))
    fmt.Printf("capacity=%d\n", cap(num2))

    //Only end
    num3 := numbers[:3]
    fmt.Println("\nOnly end")
    fmt.Printf("num1=%v\n", num3)
    fmt.Printf("length=%d\n", len(num3))
    fmt.Printf("capacity=%d\n", cap(num3))

    //None
    num4 := numbers[:]
    fmt.Println("\nOnly end")
    fmt.Printf("num1=%v\n", num4)
    fmt.Printf("length=%d\n", len(num4))
    fmt.Printf("capacity=%d\n", cap(num4))
}
```

**Output**

```
Both start and end
num1=[3 4]
length=2
capacity=3

Only start
num1=[3 4 5]
length=3
capacity=3

Only end
num1=[1 2 3]
length=3
capacity=5

Only end
num1=[1 2 3 4 5]
length=5
capacity=5
```

Here also the newly created slices refer to the same underlying array that was being referred to by the original slice.  To check this change element at any one of the index of the original slice and then reprint all the newly created slices

```
numbers[3] = 8
fmt.Printf("num1=%v\n", num2)
fmt.Printf("num3=%v\n", num3)
fmt.Printf("num4=%v\n", num4)
```

## **Using the make function**

**make** is a builtin function provided by go that can also be used to create a slice. Below is the signature of make function

```
func make([]{type}, length, capacity int) []{type}
```

Capacity is an optional parameter while creating slice using the make function. When capacity is omitted, the capacity of the slice is equal length specified for the slice. When using make function, behind the scenes go allocates an array equal to the capacity. All the elements of the allocated array are initialized with default zero value of the type. Let’s see a program illustrating this point.

```
package main

import "fmt"

func main() {
    numbers := make([]int, 3, 5)
    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))

    //With capacity ommited
    numbers = make([]int, 3)
    fmt.Println("\nCapacity Ommited")
    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))
}
```

**Output**

```
numbers=[0 0 0]
length=3
capacity=5

With Capacity Ommited
numbers=[0 0 0]
length=3
capacity=3
```

## **Using the new function**

**new** is a builtin function provided by go that can also be used to create a slice. It is not a very popular way of creating a slice as **make** is much more flexible in terms of functionalities . It is not generally used and also using **new** function returns a pointer to nil slice. Let’s see an example. In below example we are using the dereferencing operator **‘*’** as **new** function returns a pointer to the nil slice

```
package main

import "fmt"

func main() {
    numbers := new([]int)
    fmt.Printf("numbers=%v\n", *numbers)
    fmt.Printf("length=%d\n", len(*numbers))
    fmt.Printf("capacity=%d\n", cap(*numbers))
}
```

**Output**

```
numbers=[]
length=0
capacity=0
```

# **Create an array**

Both number of elements and actual elements are optional in the array declaration.

In below example, we see 4 ways of creating of an array

*   Specifying both the length of the array and actual elements. Eg.

```
[2]int{1, 2}
```

*   Only length – In this case all the actual elements are filled up with default value zero of that type. Eg

```
[2]int{}
```

*   Only actual elements – In this case, the length of the array will be equal to the number of actual elements. The symbol **‘…’** needs to be used within square brackets like this […] when not specifying the length. The symbol is an instruction to the compiler to calculate the length.

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

*   [go](https://golangbyexample.com/tag/go/)*