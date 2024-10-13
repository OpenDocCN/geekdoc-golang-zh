<!--yml

类别：未分类

日期：2024-10-13 06:39:30

-->

# 在 Go (Golang) 中将文件读入变量

> 来源：[https://golangbyexample.com/read-file-variable-golang/](https://golangbyexample.com/read-file-variable-golang/)

目录

**   [概述](#Overview "概述")

+   [使用 ioutil 包提供的 ReadFile 函数](#Using_ReadFile_function_provided_by_the_ioutil_package "使用 ioutil 包提供的 ReadFile 函数")

+   [使用 os.Open 和 bytes.Buffer](#Using_osOpen_and_then_using_bytesBuffer "使用 os.Open 和 bytes.Buffer")

+   [使用 os.Open 和 strings.Builder](#Using_osOpen_and_then_using_stringsBuilder "使用 os.Open 和 strings.Builder")*  *## **概述**

有多种方法可以在 golang 中将文件读入变量。以下是一些方法

+   使用 **ioutil** 包提供的 **ReadFile** 函数

+   使用 **os.Open** 然后使用 **bytes.Buffer**

+   使用 **os.Open** 然后使用 **strings.Builder**

另外，请注意，只有在读取小文件时，将整个文件读入变量才有意义。如果文件很大，那么逐行读取就更有意义。请参考这篇文章。

[https://golangbyexample.com/read-large-file-line-by-line-go/](https://golangbyexample.com/read-large-file-line-by-line-go/)

注意：在尝试本教程中的示例之前，请在运行程序的目录中创建一个名为 **test.png** 的文件

## **使用 ioutil 包提供的 ReadFile 函数**

[https://golang.org/pkg/io/ioutil/#ReadFile](https://golang.org/pkg/io/ioutil/#ReadFile)

下面是相应的程序

```go
package main

import (
    "fmt"
    "io/ioutil"
)

func main() {
    fileBytes, err := ioutil.ReadFile("test.png")
    if err != nil {
        panic(err)
    }
    fileString := string(fileBytes)
    fmt.Println(fileString)
}
```

**输出**

```go
Some Garbage Output depending upon the file
```

## **使用 os.Open 和 bytes.Buffer**

[https://golang.org/pkg/bytes/#Buffer](https://golang.org/pkg/bytes/#Buffer)

下面是相应的程序

```go
package main

import (
	"bytes"
	"fmt"
	"log"
	"os"
)

func main() {
	file, err := os.Open("test.png")
	if err != nil {
		log.Fatalf("Error while opening file. Err: %s", err)
	}
	defer file.Close()

	fileBuffer := new(bytes.Buffer)
	fileBuffer.ReadFrom(file)
	fileString := fileBuffer.String()

	fmt.Print(fileString)
}
```

**输出**

```go
Some Garbage Output depending upon the file
```

## 使用 **os.Open** 然后使用 **strings.Builder**

[https://golang.org/pkg/strings/#Builder](https://golang.org/pkg/strings/#Builder)

下面是相应的程序

```go
package main

import (
	"bytes"
	"fmt"
	"log"
	"os"
)

func main() {
	file, err := os.Open("test.png")
	if err != nil {
		log.Fatalf("Error while opening file. Err: %s", err)
	}
	defer file.Close()

	fileBuffer := new(bytes.Buffer)
	fileBuffer.ReadFrom(file)
	fileString := fileBuffer.String()

	fmt.Print(fileString)
}
```

**输出**

```go
Some Garbage Output depending upon the file
```

另外，查看我们的 Golang 高级教程系列 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
