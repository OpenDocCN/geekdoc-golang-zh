<!--yml

类别：未分类

日期：2024-10-13 06:29:50

-->

# 在Go（Golang）中，go.mod文件中的依赖为//indirect。

> 来源：[https://golangbyexample.com/indirect-dependency-golang/](https://golangbyexample.com/indirect-dependency-golang/)

目录

**[概述](#Overview "概述")**

+   [示例](#Example "示例")*  *# **概述**

正如名称所示，**//indirect**表示在**go.mod**文件中的依赖是间接的。**go.mod**文件仅记录直接依赖。然而，在以下情况下，它可能记录间接依赖。

+   任何未列入直接依赖的**go.mod**文件中的间接依赖，或者如果直接依赖没有**go.mod**文件，则该依赖将以**//indirect**作为后缀添加到go.mod文件中。

+   任何未在模块的任何源文件中导入的依赖。如果通过go get添加了该依赖，它将被记录为**//indirect**。

# **示例**

让我们通过示例理解上述两点。为此，首先创建一个模块。

```go
git mod init learn
```

现在创建一个文件**learn.go**。

```go
package main

import (
	"github.com/gocolly/colly"
)

func main() {
	_ = colly.NewCollector()
}
```

请注意，我们在**learn.go**中指定了该依赖。

```go
github.com/gocolly/colly
```

因此，**github.com/gocolly/colly**是**learn**模块的直接依赖，因为它在模块中被直接导入。让我们将colly版本v1.2.0作为依赖添加到go.mod文件中。

```go
module learn

go 1.14

require	github.com/gocolly/colly v1.2.0
```

现在让我们运行以下命令。

```go
go mod tidy
```

在运行该命令后，我们再次检查**go.mod**文件的内容。由于colly版本v1.2.0没有**go.mod**文件，colly所需的所有依赖将以**//indirect**作为后缀添加到**go.mod**文件中。

执行**cat go.mod**。

```go
module learn

go 1.14

require (
	github.com/PuerkitoBio/goquery v1.6.0 // indirect
	github.com/antchfx/htmlquery v1.2.3 // indirect
	github.com/antchfx/xmlquery v1.3.3 // indirect
	github.com/gobwas/glob v0.2.3 // indirect
	github.com/gocolly/colly v1.2.0
	github.com/kennygrant/sanitize v1.2.4 // indirect
	github.com/saintfish/chardet v0.0.0-20120816061221-3af4cd4741ca // indirect
	github.com/temoto/robotstxt v1.1.1 // indirect
	golang.org/x/net v0.0.0-20201027133719-8eef5233e2a1 // indirect
	google.golang.org/appengine v1.6.7 // indirect
)
```

所有其他依赖均以**//indirect**作为后缀。此外，所有直接和间接依赖的校验和将记录在go.sum文件中。

我们还提到，任何未在源文件中导入的依赖将标记为//indirect。为此，请删除上面创建的**learn.go**。同时清理**go.mod**文件以移除所有require行。现在运行以下命令。

```go
go get github.com/pborman/uuid
```

现在检查**go.mod**文件的内容。

```go
module learn

go 1.14

require github.com/pborman/uuid v1.2.1 //indirect
```

请注意，该依赖被记录为**//indirect**，因为它未在任何源文件中导入。现在创建uuid.go文件，如下所示，导入**[github.com](http://github.com)/pborman/uuid**作为依赖。

```go
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

现在进行go build。在go build后再次检查go.mod文件的内容。由于该依赖在uuid.go模块的源文件中被要求，**//indirect**将被移除。

```go
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
