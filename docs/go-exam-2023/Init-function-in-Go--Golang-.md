<!--yml

分类：未分类

日期：2024-10-13 06:30:17

-->

# Go 中的 Init 函数 (Golang)

> 来源：[`golangbyexample.com/init-function-golang/`](https://golangbyexample.com/init-function-golang/)

目录

+   概述

+   主包中的 Init 函数

+   同一包中不同源文件的多个 init() 函数 function in different source file of same package")

    +   同一源文件中的多个 init() 函数 function in same source file")

+   程序执行顺序

# **概述**

init() 函数是一个特殊函数，用于初始化包的全局变量。这些函数在包初始化时执行。一个包中的每个 GO 源文件都可以有自己的 init() 函数。每当您在程序中导入任何包时，程序执行时，属于该导入包的 GO 源文件中的 init 函数（如果存在）会首先被调用。关于 init 函数的一些要点：

+   Init 函数是可选的。

+   Init 函数不接受任何参数

+   Init 函数没有返回值。

+   Init 函数是隐式调用的。由于是隐式调用，init 函数无法从任何地方引用它。

+   同一源文件中可以有多个 init() 函数。

init 函数主要用于初始化无法使用初始化表达式初始化的全局变量。例如，它需要网络调用来初始化任何数据库客户端。另一个例子可能是在启动时获取密钥。init 函数也用于运行只需要执行一次的任何内容。让我们看一个使用 init 函数的简单用例。

假设您在应用程序中使用 redis 缓存。您希望在应用程序首次启动时创建 redis 客户端。初始化 redis 客户端的一个好地方是在 init() 函数中。让我们看一个例子。

创建一个名为 redisexample 的模块，并将以下文件放入该模块中。

**go.mod**

```go
module redisexample

go 1.14
```

**redis/redis.go**

```go
package redis

import (
	"encoding/json"
	"time"

	"github.com/go-redis/redis"
)

type redisClient struct {
	c *redis.Client
}

var (
	client = &redisClient{}
)

func init() {
	c := redis.NewClient(&redis.Options{
		Addr: "127.0.0.1:6379",
	})

	if err := c.Ping().Err(); err != nil {
		panic("Unable to connect to redis " + err.Error())
	}
	client.c = c
}

func GetKey(key string, src interface{}) error {
	val, err := client.c.Get(key).Result()
	if err == redis.Nil || err != nil {
		return err
	}
	err = json.Unmarshal([]byte(val), &src)
	if err != nil {
		return err
	}
	return nil
}

//SetKey set key
func SetKey(key string, value interface{}, expiration time.Duration) error {
	cacheEntry, err := json.Marshal(value)
	if err != nil {
		return err
	}
	err = client.c.Set(key, cacheEntry, expiration).Err()
	if err != nil {
		return err
	}
	return nil
}
```

**main.go**

```go
package main

import (
	"fmt"
	"log"
	"time"

	"redisexample/redis"
)

func main() {
	err := redis.SetKey("a", "b", time.Minute*1)
	if err != nil {
		log.Fatalf("Error: %v", err.Error())
	}

	var value string
	err = redis.GetKey("a", &value)
	if err != nil {
		log.Fatalf("Error: %v", err.Error())
	}

	fmt.Println(value)

}
```

**输出**

```go
b
```

在上述示例中，我们在 **redis.go** 文件中有一个 init 函数。在 init 函数中，我们初始化了 redis 全局客户端，该客户端在 main.go 的 **main** 函数中使用。

# **主包中的 Init 函数**

**main** 包也可以包含 init 函数。init 函数将在 **main** 函数之前触发。

```go
package main

import "fmt"

var sample int

func init() {
	fmt.Println("In Init function")
	sample = 4
}

func main() {
	fmt.Println("Main started")
	fmt.Println(sample)
	fmt.Println("Main Ended")
}
```

**输出**

```go
In Init function
Main started
4
Main Ended
```

在上述程序中，我们有一个 init 函数和一个 main 函数。在 init 函数中，我们初始化了全局变量“sample”。注意执行顺序，即 init 函数首先执行，然后执行 main 函数。

# **同一包中不同源文件的多个 init() 函数**

**init()**函数在属于某个包的所有源文件中被调用（如果存在）。让我们看一个例子。

**learn/math/add.go**

```go
package math

import "fmt"

func init(){
	fmt.Println("In add init")
}

func Add(a, b int) int {
	return a + b
}
```

**learn/math/subtract.go**

```go
package math

import "fmt"

func init(){
	fmt.Println("In subtract init")
}

func Subtract(a, b int) int {
	return a - b
}
```

**learn/main.go**

```go
package main

import (
	"fmt"

	"github.com/learn/math"
)

func main() {
	fmt.Println(math.Add(2, 1))
	fmt.Println(math.Subtract(2, 1))
}
```

**输出**

```go
In add init
In subtract init
3
1
```

## **同一源文件中的多个 init()函数**

同一源文件中可以有多个 init 函数。

```go
package main

import "fmt"

var sample int

func init() {
	fmt.Println("Init1")
}

func init() {
	fmt.Println("Init2")
}

func main() {
	fmt.Println("Main started")
	fmt.Println("Main Ended")
}
```

**输出**

```go
Init1
Init2
Main started
Main Ended
```

在上述程序中，我们有一个 init 函数和一个 main 函数。在 init 函数中，我们初始化全局变量“sample”。注意执行顺序，即 init 函数先执行，然后 main 函数执行。

# **程序的执行顺序**

现在如上所述，让我们看看 go 程序的执行顺序。

+   程序从主包开始。

+   主包源文件中导入的所有包都会被初始化。进一步导入的包同样会递归地执行。

+   然后这些包中的全局变量声明被初始化。初始化依赖关系会影响这些变量的初始化。[`golang.org/ref/spec#Order_of_evaluation`](https://golang.org/ref/spec#Order_of_evaluation)

+   之后，这些包中的 init()函数会被运行。

+   主包中的全局变量被初始化。

+   如果存在，主包中的 init 函数将被运行。

+   主包中的 main 函数被运行。

请注意，包初始化只会执行一次，即使被多次导入。

例如，如果主包导入包**a**，而包**a**又导入包**b**，则执行顺序如下：

+   包**b**中的全局变量将被初始化。包 b 的源文件中的 init 函数将被运行。

+   包**a**中的全局变量将被初始化。包**b**的源文件中的 init 函数将被运行。

+   **main**包中的全局变量将被初始化。主包源文件中的 init 函数将被运行。

+   **main**函数将开始执行。

让我们看一个相同的程序。

**go.mod**

```go
module sample

go 1.14
```

**sample/b/b1.go**

```go
package b

import (
	"fmt"
)

func init() {
	fmt.Println("Init: b1")
}

func TestB() error {
	return nil
}
```

**sample/b/b2.go**

```go
package b

import (
	"fmt"
)

func init() {
	fmt.Println("Init: b2")
}
```

**sample/a/a1.go**

```go
package a

import (
	"fmt"
	"sample/b"
)

func init() {
	fmt.Println("Init: a1")
}

func TestA() error {
	return b.TestB()
}
```

**sample/a/a2.go**

```go
package a

import (
	"fmt"
)

func init() {
	fmt.Println("Init: a2")
}
```

**sample/main.go**

```go
package main

import (
	"fmt"
	"sample/a"
)

func init() {
	fmt.Println("Init: main")
}
func main() {
	fmt.Println("Main Function Executing")
	a.TestA()
}
```

**输出**

```go
Init: b1
Init: b2
Init: a1
Init: a1
Init: main
Main Function Executing
```

请注意，在上述示例中，包**b**的源文件中的 init 函数先运行。然后包**a**的源文件中的 init 函数运行，最后是主包中的 init 函数。之后运行 main 函数。


