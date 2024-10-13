<!--yml

分类：未分类

日期：2024-10-13 06:18:55

-->

# 在 Go（Golang）中打印数组或切片元素

> 来源：[https://golangbyexample.com/print-an-array-or-slice-elements-golang/](https://golangbyexample.com/print-an-array-or-slice-elements-golang/)

目录

**[概述](#Overview "Overview")**

+   [打印切片](#Print_a_Slice "Print a Slice")

+   [打印数组](#Print_a_Array "Print a Array")*  *# **概述**

打印数组或切片的方式是相同的。让我们逐个查看

# **打印切片**

**一起打印切片元素**

使用

+   fmt.Println 和

+   fmt.Printf

```go
package main
import "fmt"
func main() {
    numbers := []int{1, 2, 3}
    fmt.Println(numbers)
    fmt.Printf("Numbers: %v", numbers)
}
```

**输出**

```go
[1 2 3]
Numbers: [1 2 3]
```

**打印单个切片元素**

使用

+   for-range 循环

```go
package main
import "fmt"
func main() {
    numbers := []int{1, 2, 3}
    for _, num := range numbers {
        fmt.Println(num)
    }
}
```

**输出**

```go
1
2
3
```

# **打印数组**

**一起打印数组元素**

使用

+   fmt.Println 和

+   fmt.Printf

```go
package main
import "fmt"
func main() {
    numbers := [3]int{1, 2, 3}
    fmt.Println(numbers)
    fmt.Printf("Numbers: %v", numbers)
}
```

**输出**

```go
[1 2 3]
Numbers: [1 2 3]
```

**打印单个数组元素**：

使用

+   for-range 循环

```go
package main
import "fmt"
func main() {
    numbers := [3]int{1, 2, 3}
    for _, num := range numbers {
        fmt.Println(num)
    }
}
```

**输出**

```go
1
2
3
```

+   [数组](https://golangbyexample.com/tag/array/)*   [切片](https://golangbyexample.com/tag/slice/)*
