<!--yml

分类：未分类

日期：2024-10-13 06:29:59

-->

# Go（Golang）中的模块供应商依赖

> 来源：[https://golangbyexample.com/vendor-dependency-go/](https://golangbyexample.com/vendor-dependency-go/)

目录

**   [概述](#Overview "Overview")

+   [示例](#Example "Example")*  *# **概述**

如果您想供应您的依赖，可以使用下面的命令来实现。

```
go mod vendor
```

# **示例**

让我们看一个示例以更好地理解。首先让我们创建一个模块

```
go mod init learn
```

该命令将在同一目录中创建一个**go.mod**文件。

```
module learn

go 1.14
```

由于这是一个空模块，尚未指定任何直接依赖。让我们在**go.mod**文件中指定一个依赖。

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

在同一目录中创建一个名为**uuid.go**的文件，内容如下，并使用我们之前在**go.mod**文件中添加的依赖。

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

让我们运行下面的命令

```
go mod vendor
```

这将创建一个vendor目录在您的项目目录中。vendor目录将包含所有直接和间接下载的依赖。您还可以将vendor目录检查到您的版本控制系统（VCS）中。这在运行时不需要下载任何依赖，因为它们已经存在于检查到VCS的vendor文件夹中。

**-v**标志也可以与go mod vendor一起使用。当提供该标志时，命令将打印所有被引入的模块和包。

```
go mod vendor -v
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
