<!--yml

类别：未分类

日期：2024-10-13 06:21:35

-->

# Go中的Hello World（Golang）

> 来源：[https://golangbyexample.com/hello-world-golang/](https://golangbyexample.com/hello-world-golang/)

让我们看看如何用golang编写一个简单的Hello World程序。创建一个扩展名为.go的文件。我们将这个文件命名为**helloworld.go**。下面是文件的内容。

```go
package main  

import "fmt" 

func main() { 
  fmt.Println("Hello World") 
}
```

关于上述程序需要注意的几点

+   每个go文件都以包名开始。在上述情况下，它是包**main**

+   只有**main**包是可执行的。

+   **main**包将包含一个**main**函数，标志着程序的开始

+   我们使用**fmt**包的**Println**函数来打印**Hello World**字符串

```go
fmt.Println("Hello World")
```

现在让我们运行这个文件。要运行，请转到包含此文件的目录。输入下面的命令来运行该文件。这个命令的作用是编译go文件并立即运行它。

```go
go run helloworld.go
```

**输出**

```go
Hello World
```
