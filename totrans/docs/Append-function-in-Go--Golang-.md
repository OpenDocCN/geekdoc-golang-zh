<!--yml
category: 未分类
date: 2024-10-13 06:18:46
-->

# Append function in Go (Golang)

> 来源：[https://golangbyexample.com/append-function-in-golang/](https://golangbyexample.com/append-function-in-golang/)

go **builtin** package provides an **append** function that can be used to append to a slice at the end. Below is the signature of this function

```
func append(slice []Type, elems ...Type) []Type
```

The first argument is the slice itself. The second is the variable number of arguments which is

```
elems ...Type
```

**‘…’** operator is the variadic syntax. So basically **…Type** means that the append function can accept a variable number of arguments of type **Type**. Below is the way for using this function. In the below code, we are appending 4 to a slice which has two elements. It appends at the end and returns the original slice. That is why we are collecting results again in **numbers** variable. It is also ok to assign the result to some other variable.

```
numbers := []int{1,2}
numbers := append(numbers, 4) //Slice will become [1, 2, 3, 4]
```

It is also ok to append many number of elements because the second argument is the variadic argument.

```
numbers := []int{1,2}
numbers := append(numbers, 3, 4, 5) //Slice will become [1, 2, 3, 4, 5]
```

This function in background increases the length and capacity of the slice. There are two cases

*   When slice length is less than capacity

In this case, on using the append function,  the length of the slice will be increased  by one without any change in its capacity. Let’s see a example

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

Capacity in all cases doesn’t changes and it is 5 while length increases by 1.

*   When slice length is equal than capacity.

In this case since there is no more capacity, so no new elements can be accommodated.  So in this case under the hood an array of double the capacity will be allocated. The current array pointed by the  slice will be copied to that new array. Now the slice will starting pointing to this new array. Hence the capacity will be doubled and length will be increased by 1\. Let’s see a example

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

Notice **‘…’** after the second slice. **‘…’** is the operator which means that the argument is a variadic parameter. Meaning that during run time slice2 will be expanded to its individual elements which are passed as multiple arguments to the append function.

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

**append function for string**

A string in go is nothing but a sequence of bytes. Hence it is legal to append a string to a slice of bytes. Below is the program for that. Notice the **‘…’** at then end of the string

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