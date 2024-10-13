<!--yml

类别：未分类

日期：2024-10-13 06:29:09

-->

# Go 中 go.mod 文件的直接与间接依赖

> 来源：[https://golangbyexample.com/direct-indirect-dependency-module-go/](https://golangbyexample.com/direct-indirect-dependency-module-go/)

目录

**   [概述](#Overview "概述")

+   [直接和间接依赖的示例](#Example_of_direct_and_indirect_dependency "直接和间接依赖的示例")

+   [go.mod 文件中的间接依赖示例](#Example_of_indirect_dependency_in_gomod_file "go.mod 文件中的间接依赖示例")*  *# **概述**

模块是 Go 对依赖管理的支持。模块的定义是一组相关包，根目录有 **go.mod**。 **go.mod** 文件定义了

+   模块导入路径。

+   模块成功构建的依赖要求。它定义了模块的依赖要求并将其锁定到正确的版本。

模块的依赖可以分为两种类型

+   **直接依赖** - 直接依赖是模块直接导入的依赖。

+   **间接依赖** - 它是模块的直接依赖所导入的依赖。此外，任何在 go.mod 文件中提到但未在模块的任何源文件中导入的依赖也被视为间接依赖。

**go.mod** 文件仅记录直接依赖。但是，在以下情况下，它可能记录间接依赖

+   任何未在你的直接依赖的 go.mod 文件中列出的间接依赖，或者如果直接依赖没有 **go.mod** 文件，那么该依赖将以 //indirect 作为后缀添加到 go.mod 文件中。

+   任何在模块的源文件中未被导入的依赖。

**go.sum** 将记录直接和间接依赖的校验和

# **直接和间接依赖的示例**

让我们来看一个直接依赖的示例。为此，首先创建一个模块

```
git mod init learn
```

现在创建一个文件 **learn.go**

```
package main

import (
	"github.com/pborman/uuid"
)

func main() {
	_ = uuid.NewRandom()
}
```

请注意，我们在 **learn.go** 中指定的依赖是

```
"github.com/pborman/uuid"
```

所以 **[github.com](http://github.com)/pborman/uuid** 是 learn 模块的直接依赖，因为它在模块中被直接导入。现在让我们运行以下命令

```
go mod tidy
```

此命令将下载你源文件中所需的所有依赖。运行此命令后，我们再检查一下 **go.mod** 文件的内容

执行 cat **go.mod**

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

它列出了在 **learn.go** 文件中指定的直接依赖以及依赖的确切版本。现在我们也来检查 go.sum 文件

执行 cat **go.sum**

```
github.com/google/uuid v1.0.0 h1:b4Gk+7WdP/d3HZH8EJsZpvV7EtDOgaZLtnaNGIu1adA=
github.com/google/uuid v1.0.0/go.mod h1:TIyPZe4MgqvfeYDBFedMoGGpEw/LqOeaOT+nhxU+yHo=
github.com/pborman/uuid v1.2.1 h1:+ZZIw58t/ozdjRaXh/3awHfmWRbzYxJoAdNJxe/3pvw=
github.com/pborman/uuid v1.2.1/go.mod h1:X/NO0urCmaxf9VXbdlT7C2Yzkj2IKimNn4k+gtPdI/k=
```

**go.sum** 文件列出了模块所需的直接和间接依赖的校验和。[github.com](http://github.com)/google/uuid 是 [github.com](http://github.com)/pborman/uuid 内部使用的，它是模块的间接依赖，因此记录在 **go.sum** 文件中。

以上方式我们在源文件中添加了一个依赖，并使用**go mod tidy**命令下载该依赖并将其添加到**go.mod**文件中。

# **go.mod文件中的间接依赖示例**。

让我们通过一个示例来理解这一点。为此，首先创建一个模块。

```
git mod init learn
```

现在创建一个文件**learn.go**。

```
package main

import (
	"github.com/gocolly/colly"
)

func main() {
	_ = colly.NewCollector()
```

请注意，我们在**learn.go**中指定了依赖，如下所示。

```
github.com/gocolly/colly
```

因此，github.com/gocolly/colly是learn模块的直接依赖，因为它在模块中直接导入。让我们将colly版本v1.2.0作为依赖添加到go.mod文件中。

```
module learn

go 1.14

require	github.com/gocolly/colly v1.2.0
```

现在让我们运行下面的命令。

```
go mod tidy
```

在运行此命令后，让我们再次检查**go.mod**文件的内容。由于colly版本v1.2.0没有go.mod文件，colly所需的所有依赖项将以**//indirect**为后缀添加到**go.mod**文件中。

执行**cat go.mod**。

```
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

所有其他依赖项都以**//indirect**为后缀。此外，所有直接和间接依赖项的校验和将记录在go.sum文件中。

我们还提到，任何未在任何源文件中导入的依赖项将标记为//indirect。为了说明这一点，删除上面创建的**learn.go**。同时清理**go.mod**文件以删除所有require行。现在运行下面的命令。

```
go get github.com/pborman/uuid
```

现在检查**go.mod**文件的内容。

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1 // indirect
```

请注意，该依赖记录为//indirect，因为它未在任何源文件中导入。

+   [go](https://golangbyexample.com/tag/go/) * [golang](https://golangbyexample.com/tag/golang/) *
