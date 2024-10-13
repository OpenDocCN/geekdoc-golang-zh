<!--yml

分类：未分类

日期：2024-10-13 06:30:03

-->

# 理解 Go（Golang）中的 go.sum 和 go.mod 文件。

> 来源：[`golangbyexample.com/go-mod-sum-module/`](https://golangbyexample.com/go-mod-sum-module/)

**注意：** 如果你有兴趣学习 Golang，那么我们有一个全面的 golang 教程系列。请查看一下 – [Golang 综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)。现在让我们看看当前的教程。下面是内容目录。

目录

**   概述

+   示例

+   go.mod 文件中的间接依赖示例 * # **概述**

模块是 go 对依赖管理的支持。根据定义，模块是一个相关包的集合，其根目录下有 **go.mod** 文件。 **go.mod** 文件定义了该模块的

+   模块导入路径。

+   创建模块时所使用的 go 版本。

+   模块成功构建的依赖要求。它定义了项目的依赖要求，并将其锁定到正确的版本。

**go.sum**

该文件列出了所需的直接和间接依赖的校验和及其版本。值得一提的是，**go.mod** 文件对于成功构建是足够的。那么为什么需要 **go.sum** 文件呢？ **go.sum** 文件中的校验和用于验证每个直接和间接依赖的校验和，以确认它们没有被修改。

我们上面提到过，go.mod 文件列出了模块的依赖要求。现在，一个模块的依赖可以分为两种类型。

+   **直接** - 直接依赖是模块直接导入的依赖。

+   **间接** – 这是模块的直接依赖所导入的依赖。此外，任何在 **go.mod** 文件中提到但未在模块的任何源文件中导入的依赖也被视为间接依赖。

**go.mod** 文件仅记录直接依赖。然而，在以下情况下，它可能记录间接依赖。

+   任何不在你的直接依赖的 **go.mod** 文件中列出的间接依赖，或者如果直接依赖没有 **go.mod** 文件，那么该依赖将以 **//indirect** 作为后缀添加到 **go.mod** 文件中。我们将在文章后面看到一个示例以便更好地理解这一点。

还请注意，**go.mod** 和 **go.sum** 文件都应当被检查到版本控制系统（VCS）中，例如 git。

# **示例**

让我们看一个例子来理解我们刚才谈到的 **go.mod** 和 **go.sum** 文件。为此，让我们先创建一个模块。

```go
go mod init learn
```

此命令将在同一目录下创建一个 **go.mod** 文件。让我们检查一下这个文件的内容。执行 cat **go.mod**。

```go
module learn

go 1.14
```

当模块使用 init 命令首次创建时，**go.mod** 文件将只包含两个内容。

+   模块的导入路径在顶部。

```go
module learn
```

+   创建模块时使用的 go 版本

```go
go 1.14
```

由于这是一个空模块，因此尚未指定任何直接依赖项。我们在同一目录中创建一个名为**uuid.go**的文件，内容如下：

**uuid.go**

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

请注意，我们在 uuid.go 中也导入了依赖项。

```go
"github.com/pborman/uuid"
```

让我们运行以下命令：

```go
go mod tidy
```

此命令将下载源文件中所需的所有依赖项，并用该依赖项更新**go.mod**文件。运行此命令后，让我们再次检查**go.mod**文件的内容。执行`cat go.mod`

```go
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

它列出了在 uuid 文件中指定的直接依赖项及其确切版本。现在让我们检查一下**go.sum**文件。

执行`cat go.sum`

```go
github.com/google/uuid v1.0.0 h1:b4Gk+7WdP/d3HZH8EJsZpvV7EtDOgaZLtnaNGIu1adA=
github.com/google/uuid v1.0.0/go.mod h1:TIyPZe4MgqvfeYDBFedMoGGpEw/LqOeaOT+nhxU+yHo=
github.com/pborman/uuid v1.2.1 h1:+ZZIw58t/ozdjRaXh/3awHfmWRbzYxJoAdNJxe/3pvw=
github.com/pborman/uuid v1.2.1/go.mod h1:X/NO0urCmaxf9VXbdlT7C2Yzkj2IKimNn4k+gtPdI/k=
```

**go.sum**文件列出了模块所需的直接和间接依赖项的校验和。**[github.com](http://github.com)/google/uuid**被**[github.com](http://github.com)/pborman/uuid**内部使用。它是该模块的间接依赖项，因此记录在**go.sum**文件中。

我们现在也可以运行这个文件，它将给出正确的输出。

```go
go run uuid.go
```

**输出**

```go
e594dc4d9a754bcb83b56e89b18b4b46
```

# **go.mod 文件中的间接依赖示例**

我们在上面提到，**go.mod**文件在以下情况下可能包含间接依赖项。

+   任何未在直接依赖项的**go.mod**文件中列出的间接依赖项，或者如果直接依赖项没有 go.mod 文件，那么该依赖项将以**//indirect**作为后缀添加到**go.mod**文件中。我们将在本文后面看到一个示例以更好地了解这一点。

让我们通过一个示例来理解。为此，我们先再次创建一个模块。

```go
git mod init learn
```

现在创建一个文件**learn.go**

```go
package main

import (
	"github.com/gocolly/colly"
)

func main() {
	_ = colly.NewCollector()
}
```

请注意，我们在**learn.go**中指定的依赖项如下：

```go
github.com/gocolly/colly
```

因此，**github.com/gocolly/colly**是 learn 模块的直接依赖项，因为它在模块中被直接导入。现在让我们运行以下命令：

```go
go mod tidy
```

运行此命令后，让我们再次检查**go.mod**文件的内容。由于 colly 版本 v1.2.0 没有 go.mod 文件，colly 所需的所有依赖项将以//indirect 作为后缀添加到**go.mod**文件中。

执行`cat go.mod`

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

所有其他依赖项都以**//indirect**结尾。所有直接和间接依赖项的校验和将记录在 go.sum 文件中。

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/) *
