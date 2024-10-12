<!--yml
category: 未分类
date: 2024-10-13 06:39:40
-->

# Append or Add to a Slice or Array in Go (Golang)

> 来源：[https://golangbyexample.com/append-slice-array-golang/](https://golangbyexample.com/append-slice-array-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [append() Function](#append_Function "append() Function")
    *   [When slice length is less than capacity](#When_slice_length_is_less_than_capacity "When slice length is less than capacity")
    *   [When slice length is equal to the capacity](#When_slice_length_is_equal_to_the_capacity "When slice length is equal to the capacity")
*   [append() Function for string](#append_Function_for_string "append() Function for string")*  *## **Overview**

In golang, the size of the **array** is part of its type. That is why the length of the array is fixed during creation and cannot be changed later.  This is where the **slice** comes into the picture. Slice is more powerful and convenient to use than an array.  Slice, in fact, is more analogous to arrays in another programming language. In this tutorial, we will look at how we can add or append to a slice.

Internally a slice is represented by three things.

*   Pointer to the underlying array

*   The current length of the underlying array

*   Total Capacity which is the maximum capacity to which the underlying array can expand.

Read more about slice here – [https://golangbyexample.com/slice-in-golang/](https://golangbyexample.com/slice-in-golang/)

## **append() Function**

go **builtin** package provides an **append** function that can be used to append or add to a slice at the end. Below is the signature of this function

```
func append(slice []Type, elems ...Type) []Type
```

The first argument is the slice itself. The second is the variable number of arguments which is

```
elems ...Type
```

**‘…’** operator is the variadic syntax. So basically **…Type** means that the append function can accept a variable number of arguments of type **Type**. Below is the way for using this function. In the below code, we are appending 4 to a slice that has two elements. It appends at the end and returns the original slice. That is why we are collecting results again in the **numbers** variable. It is also ok to assign the result to some other variable.

```
numbers := []int{1,2}
numbers := append(numbers, 4) //Slice will become [1, 2, 3, 4]
```

It is also ok to append any number of elements because the second argument is the variadic argument.

```
numbers := []int{1,2}
numbers := append(numbers, 3, 4, 5) //Slice will become [1, 2, 3, 4, 5]
```

This function in the background increases the length and capacity of the slice. There are two cases

*   When slice length is less than capacity

*   When slice length is equal to capacity

### **When slice length is less than capacity**

In this case, by using the append function,  the length of the slice will be increased by one without any change in its capacity. Let’s see an example

```
package main

import "fmt"

func main() {
    numbers := make([]int, 3, 5)
    numbers[0] = 1
    numbers[1] = 2
    numbers[2] = 3
    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))

    //Append number 4
    numbers = append(numbers, 4)
    fmt.Println("\nAppend Number 4")
    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))

    //Append number 5
    numbers = append(numbers, 4)
    fmt.Println("\nAppend Number 5")
    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))
}
```

**Output**

```
numbers=[1 2 3]
length=3
capacity=5

Append Number 4
numbers=[1 2 3 4]
length=4
capacity=5

Append Number 5
numbers=[1 2 3 4 4]
length=5
capacity=5
```

Capacity in all cases doesn’t change and it is 5 while length increases by 1.

### **When slice length is equal to the capacity**

In this case, since there is no more capacity, so no new elements can be accommodated.  So in this case under the hood, an array of double the capacity will be allocated. The current array pointed by the slice will be copied to that new array. Now the slice will start pointing to this new array. Hence the capacity will be doubled and length will be increased by 1\. Let’s see an example

```
package main

import "fmt"

func main() {
    numbers := make([]int, 3, 3)
    numbers[0] = 1
    numbers[1] = 2
    numbers[2] = 3

    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))

    //Append number 4
    numbers = append(numbers, 4)
    fmt.Println("\nAppend Number 4")
    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))
}
```

**Output**

```
numbers=[1 2 3]
length=3
capacity=3

Append Number 4
numbers=[1 2 3 4]
length=4
capacity=6
```

Notice in the above example that the capacity is doubled.

It is also possible to append one slice to another slice. Below is the format for that.

```
res := append(slice1, slice2...)
```

Notice **‘…’** after the second slice.** ‘…’ **is the operator which means that the argument is a variadic parameter. Meaning that during run time slice2 will be expanded to its individual elements which are passed as multiple arguments to the append function.

```
package main

import "fmt"

func main() {
    numbers1 := []int{1, 2}
    numbers2 := []int{3, 4}
    numbers := append(numbers1, numbers2...)
    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))
}
```

**Output**

```
numbers=[1 2 3 4]
length=4
capacity=4
```

## **append() Function for string**

A string in go is nothing but a sequence of bytes. Hence it is legal to append a string to a slice of bytes. Below is the program for that. Notice the **‘…’** at the end of the string

```
package main

import "fmt"

func main() {
    sample := "Hello"
    suffix := "World"

    result := append([]byte(sample), suffix...)
    fmt.Printf("sample: %s\n", string(result))
}
```

**Output**

```
sample: HelloWorld
```

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*