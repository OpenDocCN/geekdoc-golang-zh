<!--yml
category: 未分类
date: 2024-10-13 06:29:59
-->

# Vendor dependencies of a module in Go (Golang)

> 来源：[https://golangbyexample.com/vendor-dependency-go/](https://golangbyexample.com/vendor-dependency-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

If you want to vendor your dependencies,  then below command can be used to achieve the same.

```
go mod vendor
```

# **Example**

Let’s see an example to better understand. First let’s create a module

```
go mod init learn
```

This command will create a **go.mod** file in the same directory.

```
module learn

go 1.14
```

Since it is an empty module it doesn’t have any direct dependency specified yet. Let’s specify a dependency in the **go.mod** file

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

Create a file named **uuid.go** in the same directory with the below contents and use the dependency we added earlier in the **go.mod** file.

**uuid.go**

```
package main

import (
	"fmt"
	"strings"

	"github.com/pborman/uuid"
)

func main() {
	uuidWithHyphen := uuid.NewRandom()
	uuid := strings.Replace(uuidWithHyphen.String(), "-", "", -1)
	fmt.Println(uuid)
}
```

Let’s run the below command

```
go mod vendor
```

It will create a vendor directory inside your project directory. The vendor directory will have all the direct and indirect dependencies downloaded. You can also check in the vendor directory to your VCS (Version Control System). This becomes useful in the sense that none of the dependency needs to be downloaded at run time as it is already present in the vendor folder checked into VCS.

**-v** flag can also be used with go mod vendor. When this flag is provided the command will print all the modules and packages which are vendored

```
go mod vendor -v
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*