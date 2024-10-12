<!--yml
category: 未分类
date: 2024-10-13 06:17:47
-->

# Get current Username in Go (Golang)

> 来源：[https://golangbyexample.com/get-current-username-golang/](https://golangbyexample.com/get-current-username-golang/)

**Overview**

**‘os/user’** package can be used to get the current user name.

Let’s see a working code

**Code**

```
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

**Output**

```
Username: "someusername"
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [username](https://golangbyexample.com/tag/username/)