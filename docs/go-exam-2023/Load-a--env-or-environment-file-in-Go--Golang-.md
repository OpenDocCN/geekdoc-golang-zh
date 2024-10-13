<!--yml

分类：未分类

日期：2024-10-13 06:41:18

-->

# 在 Go（Golang）中加载 .env 或环境文件。

> 来源：[https://golangbyexample.com/load-env-fiie-golang/](https://golangbyexample.com/load-env-fiie-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program") *  *# **概述**

**gotenv** 包在 Golang 中可用于加载 **.env** 或 **环境** 文件。

[https://github.com/joho/godotenv](https://github.com/joho/godotenv)

以下是函数的签名。

```go
func Load(filenames ...string) (err error) 
```

它接受可变数量的参数，每个参数可以是需要加载的 **.env** 文件名。

# **程序**

创建一个 **local.env** 文件，并包含以下内容。

```go
STACK=DEV
DATABASE=SQL
```

这是程序。

```go
package main

import (
	"fmt"
	"log"
	"os"

	"github.com/joho/godotenv"
)

func main() {
	err := godotenv.Load("local.env")
	if err != nil {
		log.Fatalf("Some error occured. Err: %s", err)
	}

	val := os.Getenv("STACK")
	fmt.Println(val)

	val = os.Getenv("DATABASE")
	fmt.Println(val)
}
```

**输出**

```go
DEV
SQL
```

它加载 **local.env** 文件并给出正确的输出。

它还可以用于加载多个 .env 文件。创建一个新的文件 **test.env**，并包含以下内容。

```go
TEST=UNIT
```

```go
package main

import (
	"fmt"
	"log"
	"os"

	"github.com/joho/godotenv"
)

func main() {
	err := godotenv.Load("local.env", "test.env")
	if err != nil {
		log.Fatalf("Some error occured. Err: %s", err)
	}

	val := os.Getenv("STACK")
	fmt.Println(val)

	val = os.Getenv("DATABASE")
	fmt.Println(val)

	val = os.Getenv("TEST")
	fmt.Println(val)
}
```

**输出**

```go
DEV
SQL
UNIT
```

如果你没有向 **Load** 函数提供任何参数，则默认情况下它将加载当前目录中的 **.env** 文件。

在当前目录中创建一个名为 **.env** 的文件，并包含以下内容。

```go
STACK=DEV
DATABASE=SQL
```

```go
package main

import (
	"fmt"
	"log"
	"os"

	"github.com/joho/godotenv"
)

func main() {
	err := godotenv.Load()
	if err != nil {
		log.Fatalf("Some error occured. Err: %s", err)
	}

	val := os.Getenv("STACK")
	fmt.Println(val)

	val = os.Getenv("DATABASE")
	fmt.Println(val)
}
```

请注意，在上述程序中，我们没有向 **Load** 函数传递任何参数。

**输出**

```go
DEV
SQL
```

这部分是关于在 Golang 中加载 **.env** 文件的。

**注意：** 请查看我们的 Golang 高级教程。本系列教程详尽，我们尝试涵盖所有概念和示例。本教程适合希望深入理解 Golang 的人——[Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是，那么这篇文章适合你——[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/)*
