<!--yml
category: 未分类
date: 2024-10-13 06:18:55
-->

# Print an array or slice elements in Go (Golang)

> 来源：[https://golangbyexample.com/print-an-array-or-slice-elements-golang/](https://golangbyexample.com/print-an-array-or-slice-elements-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Print a Slice](#Print_a_Slice "Print a Slice")
*   [Print a Array](#Print_a_Array "Print a Array")*  *# **Overview**

The way we print an array or slice is same. Let’s look at both one by one

# **Print a Slice**

**Print slice elements together**

Using

*   fmt.Println and
*   fmt.Printf

```
package main
import "fmt"
func main() {
    numbers := []int{1, 2, 3}
    fmt.Println(numbers)
    fmt.Printf("Numbers: %v", numbers)
}
```

**Output**

```
[1 2 3]
Numbers: [1 2 3]
```

**Print individual slice elements**

Using

*   for-range loop

```
package main
import "fmt"
func main() {
    numbers := []int{1, 2, 3}
    for _, num := range numbers {
        fmt.Println(num)
    }
}
```

**Output**

```
1
2
3
```

# **Print a Array**

**Print array elements together**

Using

*   fmt.Println and
*   fmt.Printf

```
package main
import "fmt"
func main() {
    numbers := [3]int{1, 2, 3}
    fmt.Println(numbers)
    fmt.Printf("Numbers: %v", numbers)
}
```

**Output**

```
[1 2 3]
Numbers: [1 2 3]
```

**Print individual array elements**:

Using

*   for-range loop

```
package main
import "fmt"
func main() {
    numbers := [3]int{1, 2, 3}
    for _, num := range numbers {
        fmt.Println(num)
    }
}
```

**Output**

```
1
2
3
```

*   [array](https://golangbyexample.com/tag/array/)*   [slice](https://golangbyexample.com/tag/slice/)*