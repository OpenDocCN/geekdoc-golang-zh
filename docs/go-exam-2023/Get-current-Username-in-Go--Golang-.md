<!--yml

分类：未分类

日期：2024-10-13 06:17:47

-->

# 在 Go (Golang) 中获取当前用户名

> 来源：[https://golangbyexample.com/get-current-username-golang/](https://golangbyexample.com/get-current-username-golang/)

**概述**

**‘os/user’** 包可用于获取当前用户名。

让我们看一段可运行的代码

**代码**

```go
package main

import (
	"fmt"
	"log"
	"os/user"
)

func main() {

	user, err := user.Current()
	if err != nil {
		log.Fatalf(err.Error())
	}

	username := user.Username

	fmt.Printf("Username: %s\n", username)
} 
```

**输出**

```go
Username: "someusername"
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [用户名](https://golangbyexample.com/tag/username/)
