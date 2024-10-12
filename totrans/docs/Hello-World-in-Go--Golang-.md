<!--yml
category: 未分类
date: 2024-10-13 06:21:35
-->

# Hello World in Go (Golang)

> 来源：[https://golangbyexample.com/hello-world-golang/](https://golangbyexample.com/hello-world-golang/)

Let’s see how to write a simple Hello World program in golang. Create a file with an extension .go. Let’s name this file **helloworld.go**. Below will be the contents of the file.

```
package main  

import "fmt" 

func main() { 
  fmt.Println("Hello World") 
}
```

Some points to note about above program

*   Every go file starts with a package name. In the above case it is package **main**

*   Only **main** package is executable.

*   The **main** package will contain a **main** function that denotes the start of a program

*   We are using the **Println** function of **fmt** package to print the **Hello World** string

```
fmt.Println("Hello World")
```

Let’s run this file now. For running, go to the directory which contains this file. Type below command to run the file. What this command will do is compile the go file and immediately run it.

```
go run helloworld.go
```

**Output**

```
Hello World
```