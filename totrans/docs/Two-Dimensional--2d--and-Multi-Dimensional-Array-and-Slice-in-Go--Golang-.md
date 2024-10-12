<!--yml
category: 未分类
date: 2024-10-13 06:18:33
-->

# Two Dimensional (2d) and Multi-Dimensional Array and Slice in Go (Golang)

> 来源：[https://golangbyexample.com/two-dimensional-array-slice-golang/](https://golangbyexample.com/two-dimensional-array-slice-golang/)

In go multi-dimension is possible for both array and slice. Let’s see both of them in detail.

Table of Contents

 **   [Multi-Dimensional Arrays](#Multi-Dimensional_Arrays "Multi-Dimensional Arrays")
    *   [Overview](#Overview "Overview")
    *   [Accessing elements of a multi dimensional array](#Accessing_elements_of_a_multi_dimensional_array "Accessing elements of a multi dimensional array")
    *   [Traversal of a multidimensional array](#Traversal_of_a_multidimensional_array "Traversal of a multidimensional array")
    *   [How multidimensional array is stored in memory](#How_multidimensional_array_is_stored_in_memory "How multidimensional array is stored in memory")
*   [Multi-dimensional Slices](#Multi-dimensional_Slices "Multi-dimensional Slices")
    *   [Overview](#Overview-2 "Overview")
    *   [Accessing Multi-Dimensional Slice elements](#Accessing_Multi-Dimensional_Slice_elements "Accessing Multi-Dimensional Slice elements")
    *   [Traversal of a multi dimensional slice.](#Traversal_of_a_multi_dimensional_slice "Traversal of a multi dimensional slice. ")
    *   [How multidimensional slice is stored in memory](#How_multidimensional_slice_is_stored_in_memory "How multidimensional slice is stored in memory")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Multi-Dimensional Arrays**

## **Overview**

Below is the format for declaring a multidimensional dimensional array

```
[len1][len2][len3]....[lenN]T{}
```

where

*   len1 , len2 .. lenN are length of each of the dimensions

*   T is the data type

All the rules that apply to the one-dimensional array also apply to the multidimensional array as well. It is also possible to specify the array elements during the declaration. In case the array elements are not specified during declaration, then all the array elements are allotted the default zero value of the **<data_type>**

Below is the format for declaring a two dimensional array with array elements specified.

```
var sample = [len1][len2]T{{a11, a12 .. a1y},
                       {a21, a22 .. a2y},
                       {.. },
                       {ax1, ax2 .. axy}} 
```

where

*   **len1** denotes the number of rows
*   **len2** denotes the number of columns
*   **aij** denotes an element present at i row and j column
*   **T** is the data type

Let’s see a small example illustrating above points:

```
package main

import "fmt"

func main() {

    sample := [2][3]int{{1, 2, 3}, {4, 5, 6}}

    fmt.Printf("Number of rows in array: %d\n", len(sample))
    fmt.Printf("Number of columns in array: %d\n", len(sample[0]))
    fmt.Printf("Total number of elements in array: %d\n", len(sample)*len(sample[0]))

    fmt.Println("Traversing Array")
    for _, row := range sample {
        for _, val := range row {
            fmt.Println(val)
        }
    }
}
```

**Output**

```
Number of rows in array: 2
Number of columns in array: 3
Total number of elements in array: 6
Traversing Array
1
2
3
4
5
6
```

Notice in the above program how are we able to get the number of rows, column and also the total number of elements in the array

*   Number of rows = len(sample)

*   Number of columns = len(sample[0])

*   Number of total elements = len(sample)*len(sample[0])

The same idea can be extended to three dimensions, four dimensions, and so on. Let’s see a small example of three dimensional array as well. In below program we are creating a 2*2*3 dimensional array.

```
package main

import "fmt"

func main() {
    sample := [2][2][3]int{{{1, 2, 3}, {4, 5, 6}}, {{7, 8, 9}, {10, 11, 12}}}

    fmt.Printf("Length of first dimension: %d\n", len(sample))
    fmt.Printf("Length of second dimension: %d\n", len(sample[0]))
    fmt.Printf("Length of third dimension: %d\n", len(sample[0][0]))

    fmt.Printf("Overall Dimension of the array: %d*%d*%d\n", len(sample), len(sample[0]), len(sample[0][0]))
    fmt.Printf("Total number of elements in array: %d\n", len(sample)*len(sample[0])*len(sample[0][0]))

    for _, first := range sample {
        for _, second := range first {
            for _, value := range second {
                fmt.Println(value)
            }
        }
    }
}
```

**Output**

```
Length of first dimension: 2
Length of second dimension: 2
Length of third dimension: 3
Overall Dimension of the array: 2*2*3
Total number of elements in array: 12
1
2
3
4
5
6
7
8
9
10
11
12
```

## **Accessing elements of a multi dimensional array**

An element of a multi-dimensional array can be accessed using the index of each of the dimensions. For example, a two-dimensional array can be accessed by provided its row index and column index. Once we are able to access them using the index of each of the dimensions, then it is also possible to assign a new value to it as well. Let’s see a program

```
package main

import "fmt"

func main() {
    sample := [2][3]int{{1, 2, 3}, {4, 5, 6}}
    //Print array element
    fmt.Println(sample[0][0])
    fmt.Println(sample[0][1])
    fmt.Println(sample[0][2])
    fmt.Println(sample[1][0])
    fmt.Println(sample[1][1])
    fmt.Println(sample[1][2])

    //Assign new values
    sample[0][0] = 6
    sample[0][1] = 5
    sample[0][2] = 4
    sample[1][0] = 3
    sample[1][1] = 2
    sample[1][2] = 1

    fmt.Println()
    fmt.Println(sample[0][0])
    fmt.Println(sample[0][1])
    fmt.Println(sample[0][2])
    fmt.Println(sample[1][0])
    fmt.Println(sample[1][1])
    fmt.Println(sample[1][2])
}
```

**Output**

```
1
2
3
4
5
6

6
5
4
3
2
1
```

## **Traversal of a multidimensional array**

A multidimensional array can be traversed using:

*   for-range loop
*   for loop

Let’s see a code example for traversal of a two dimensional array.

```
package main

import "fmt"

func main() {
    sample := [2][3]int{{1, 2, 3}, {4, 5, 6}}
    fmt.Println("Using for-range")
    for _, row := range sample {
        for _, val := range row {
            fmt.Println(val)
        }
    }

    fmt.Println("\nUsing for loop")
    for i := 0; i < 2; i++ {
        for j := 0; j < 3; j++ {
            fmt.Println(sample[i][j])
        }
    }

    fmt.Println("\nUsing for loop - Second way")
    for i := 0; i < len(sample); i++ {
        for j := 0; j < len(sample[i]); j++ {
            fmt.Println(sample[i][j])
        }
    }
}
```

**Output**

```
Using for-rage
1
2
3
4
5
6

Using for loop
1
2
3
4
5
6

Using for loop 
1
2
3
4
5
6
```

Some points to note about above program

*   We have to use a nested range for traversal using a for-range loop. The first range traverses each of the rows. The second range traverses the individual array present at that row

*   The same goes for iterating using for loop.

*   len(sample) gives the number of rows.

*   len(sample[i]) gives the number of columns present at row i.

*   The same idea can be extended to three dimensional, four-dimensional array element too.

## **How multidimensional array is stored in memory**

Memory allocated for array is contiguous irrespective of weather an array is one dimensional or two dimensional. For example in case of two dimension array the second row starts in memory where the first row ends. Let's see a program illustrating this point

```
package main

import "fmt"

func main() {
    sample := [2][3]byte{}

    fmt.Println("First row")
    fmt.Println(&sample[0][0])
    fmt.Println(&sample[0][1])
    fmt.Println(&sample[0][2])

    fmt.Println("\nSecond row")
    fmt.Println(&sample[1][0])
    fmt.Println(&sample[1][1])
    fmt.Println(&sample[1][2])
}
```

**Output**

```
First row
0xc0000b4002
0xc0000b4003
0xc0000b4004

Second row
0xc0000b4005
0xc0000b4006
0xc0000b4007
```

Notice all the address are contiguous. And the second row starts where the first row ends.

# **Multi-dimensional Slices**

## **Overview**

As the multidimensional array is an array of arrays, similarly multi-dimensional slice is a slice of slices. To understand this, let's first look at the definition of a slice. A slice points to an underlying array and is internally represented by a slice header. A slice header is a struct which looks like  this

```
type SliceHeader struct {
        Data uintptr
        Len  int
        Cap  int
}
```

**Data** field in slice header is pointer to the underlying array. For a one dimensional slice, we have below declaration

```
oneDSlice := make([]int, 2)
```

To declare a two dimensional slice the declaration would be

```
twoDSlice := make([][]int, 2)
```

Above declaration means that we want to create a **slice** of 2 slices. Carefully understand this point. But wait a second here, we haven't specified the second dimension here, meaning what is the length of each of the inner 2 slices. In case of slice, each of the inner slice has to be explicitly intialised like below

```
for i := range twoDSlice {
    twoDSlice[i] = make([]int, 3)
}
```

So using range on the original slice, we specify the length each of 2 slices using make.  Below is one other way of doing the same but with slice elements specified

```
var twoDSlice = make([][]int, 2)
twoDSlice[0] = []int{1, 2, 3}
twoDSlice[1] = []int{4, 5, 6}
```

Basically, with the above declaration, we create a slice of 2*3 dimensions which is a two-dimensional slice. The same idea can be extended to two-dimension, three-dimension, and so on.

A complete working example of above two points

```
package main

import "fmt"

func main() {
    twoDSlice1 := make([][]int, 3)
    for i := range twoDSlice1 {
        twoDSlice1[i] = make([]int, 3)
    }
    fmt.Printf("Number of rows in slice: %d\n", len(twoDSlice1))
    fmt.Printf("Number of columns in arsliceray: %d\n", len(twoDSlice1[0]))
    fmt.Printf("Total number of elements in slice: %d\n", len(twoDSlice1)*len(twoDSlice1[0]))
    fmt.Println("First Slice")
    for _, row := range twoDSlice1 {
        for _, val := range row {
            fmt.Println(val)
        }
    }
    twoDSlice2 := make([][]int, 2)
    twoDSlice2[0] = []int{1, 2, 3}
    twoDSlice2[1] = []int{4, 5, 6}
    fmt.Println()
    fmt.Printf("Number of rows in slice: %d\n", len(twoDSlice2))
    fmt.Printf("Number of columns in arsliceray: %d\n", len(twoDSlice2[0]))
    fmt.Printf("Total number of elements in slice: %d\n", len(twoDSlice2)*len(twoDSlice2[0]))
    fmt.Println("Second Slice")
    for _, row := range twoDSlice2 {
        for _, val := range row {
            fmt.Println(val)
        }
    }
}
```

**Output**

```
Number of rows in slice: 2
Number of columns in arsliceray: 3
Total number of elements in slice: 6
First Slice
0
0
0
0
0
0

Number of rows in slice: 2
Number of columns in arsliceray: 3
Total number of elements in slice: 6
Second Slice
1
2
3
4
5
6
```

We mentioned above that we are creating a two-dimensional slice of 2*3 dimensions.  With that said the thought that might be coming to your mind is whether it is possible to have different lengths for inner slices. Yes, it is possible. Unlike arrays which have inner arrays of the same length, in case of slice since we initialize each of the inner slices individually, it is possible to have different length for inner slices

Let's see an example

```
package main

import "fmt"

func main() {
    twoDSlice := make([][]int, 2)
    twoDSlice[0] = []int{1, 2, 3}
    twoDSlice[1] = []int{4, 5}

    fmt.Printf("Number of rows in slice: %d\n", len(twoDSlice))
    fmt.Printf("Len of first row: %d\n", len(twoDSlice[0]))
    fmt.Printf("Len of second row: %d\n", len(twoDSlice[1]))
    fmt.Println("Traversing slice")
    for _, row := range twoDSlice {
        for _, val := range row {
            fmt.Println(val)
        }
    }
}
```

**Output**

```
Number of rows in slice: 2
Len of first row: 3
Len of second row: 2
Traversing slice
1
2
3
4
5
```

Let's see a small example of a three-dimensional slice as well. In the below program, we are creating a slice of 2*2*3 dimensions.

```
package main

import "fmt"

func main() {
    sample := make([][][]int, 2)
    for i := range sample {
        sample[i] = make([][]int, 2)
        for j := range sample[i] {
            sample[i][j] = make([]int, 3)
        }
    }

    fmt.Printf("Length of first dimension: %d\n", len(sample))
    fmt.Printf("Length of second dimension: %d\n", len(sample[0]))
    fmt.Printf("Length of third dimension: %d\n", len(sample[0][0]))
    fmt.Printf("Overall Dimension of the slice: %d*%d*%d\n", len(sample), len(sample[0]), len(sample[0][0]))
    fmt.Printf("Total number of elements in slice: %d\n", len(sample)*len(sample[0])*len(sample[0][0]))
    for _, first := range sample {
        for _, second := range first {
            for _, value := range second {
                fmt.Println(value)
            }
        }
    }
}
```

**Output**

```
Length of first dimension: 2
Length of second dimension: 2
Length of third dimension: 3
Overall Dimension of the slice: 2*2*3
Total number of elements in slice: 12
0
0
0
0
0
0
0
0
0
0
0
```

## **Accessing Multi-Dimensional Slice elements**

Accessing slice elements is the same as accessing elements of an array. Let's see an example

```
package main

import "fmt"

func main() {
    sample := make([][]int, 2)
    sample[0] = []int{1, 2, 3}
    sample[1] = []int{4, 5, 6}

    //Print array element
    fmt.Println(sample[0][0])
    fmt.Println(sample[0][1])
    fmt.Println(sample[0][2])
    fmt.Println(sample[1][0])
    fmt.Println(sample[1][1])
    fmt.Println(sample[1][2])

    //Assign new values
    sample[0][0] = 6
    sample[0][1] = 5
    sample[0][2] = 4
    sample[1][0] = 3
    sample[1][1] = 2
    sample[1][2] = 1

    fmt.Println()
    fmt.Println(sample[0][0])
    fmt.Println(sample[0][1])
    fmt.Println(sample[0][2])
    fmt.Println(sample[1][0])
    fmt.Println(sample[1][1])
    fmt.Println(sample[1][2])
}
```

**Output**

```
1
2
3
4
5
6

6
5
4
3
2
1
```

## **Traversal of a multi dimensional slice.**

Traversing a multidimensional slice is the same as traversing a multi-dimensional array. A multidimensional slice can be traversed using

*   for-range loop
*   for loop

Let's see an example:

```
package main

import "fmt"

func main() {
    sample := make([][]int, 2)
    sample[0] = []int{1, 2, 3}
    sample[1] = []int{4, 5, 6}
    fmt.Println("Using for-range")
    for _, row := range sample {
        for _, val := range row {
            fmt.Println(val)
        }
    }

    fmt.Println("\nUsing for loop - Second way")
    for i := 0; i < len(sample); i++ {
        for j := 0; j < len(sample[i]); j++ {
            fmt.Println(sample[i][j])
        }
    }
}
```

**Output**

## **How multidimensional slice is stored in memory**

Since in the case of the slice, each of the inner slices is initialized separately hence it is possible that inner slice might not be contiguous in memory with respect to each other. Although each of the elements within each of inner slice will be at the contiguous location. Let's see a program illustrating this point.

```
package main

import "fmt"

func main() {
    sample := make([][]byte, 2)
    sample[0] = make([]byte, 3)

    //testVariable := "s"
    //fmt.Println(testVariable)

    sample[1] = make([]byte, 3)

    fmt.Println("First row")
    fmt.Println(&sample[0][0])
    fmt.Println(&sample[0][1])
    fmt.Println(&sample[0][2])

    fmt.Println("\nSecond row")
    fmt.Println(&sample[1][0])
    fmt.Println(&sample[1][1])
    fmt.Println(&sample[1][2])
}
```

**Output**

```
First row
0xc000018072
0xc000018073
0xc000018074

Second row
0xc000018080
0xc000018081
0xc000018082
```

**Please Note:** There is a caveat in the above program. Since the second inner slice is initialized just after the first inner slice, there is a possibility that the address allotted to both of them is contiguous. It can happen but not always. Uncomment the line for test variable and then both the inner slice will not be allotted contiguous address. In the case of an array, all inner arrays will be stored at contiguous locations always.

# **Conclusion**

This is all about multidimensional array and slice in golang. I hope you have liked this article. Please share the feedback in the comments.

*   [2d](https://golangbyexample.com/tag/2d/)*   [3d](https://golangbyexample.com/tag/3d/)*   [array](https://golangbyexample.com/tag/array/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [slice](https://golangbyexample.com/tag/slice/)*