<!--yml

类别：未分类

日期：2024-10-13 06:30:08

-->

# Go 中的包与模块（Golang）

> 来源：[https://golangbyexample.com/package-vs-module-golang/](https://golangbyexample.com/package-vs-module-golang/)

目录

**   [概述](#Overview "Overview")

+   [模块时代之前](#Before_Modules_World "Before Modules World")

    +   [Go 版本 1.11 之前](#Pre_Go_version_111 "Pre Go version 1.11")

    +   [在 Go 版本 1.11 中](#In_Go_version_111 "In Go version 1.11")

    +   [在 Go 版本 1.13 之后](#After_Go_Version_113 "After Go Version 1.13")

+   [创建模块](#Creating_Modules "Creating Modules")*  *# **概述**

根据模块的定义，它是一个包含嵌套和相关的 Go 包集合的目录，根目录下有 **go.mod** 文件。**go.mod** 文件定义了

+   模块导入路径。

+   模块的依赖要求，以确保成功构建。它定义了项目的依赖要求，并将其锁定到正确的版本。

模块提供了

+   依赖管理

+   使用模块后，Go 项目不必再位于 **$GOPATH/src** 文件夹中。

此外，除了 **go.mod** 文件，Go 还会保留一个 **go.sum** 文件，其中包含所有项目依赖模块的加密哈希。这是为了确保项目的依赖模块没有发生变化。

模块内包的行为与之前相同。因此，之前适用于包的规则现在也适用。这一点没有改变。然而，当需要单独版本化时，可以将一组包称为模块。此外，当它是通用代码并且你想在多个项目之间共享这段代码时，也是如此。

# **模块时代之前**

让我们按版本查看变化，以充分理解早期的限制以及自模块以来的变化。

+   Go 版本 1.11 之前 – 模块根本不存在。

+   Go 版本 1.11 – 引入了模块，但尚未最终确定。

+   Go 版本 1.13 – 引入了模块。

## **Go 版本 1.11 之前**

在模块出现之前，Go 只有包。$GOPATH 位置将包含三个目录。

+   src

+   pkg

+   bin

这些是在模块时代之前存在的问题。

+   所有 Go 项目位于 $GOPATH/src 目录中。

+   没有原生的依赖管理支持。

+   所有依赖项将在 $GOPATH/src 目录中下载，而没有版本控制。

让我们逐个看待每个问题。

+   任何 Go 项目都必须在 $GOPATH/src 目录内。

这是一个重大限制，因为它限制了你可以存放项目的位置。

+   没有原生的依赖管理支持。

在模块出现之前的一个问题是没有办法在项目中指定依赖。虽然有像dep、glide这样的替代解决方案，但缺乏原生解决方案。

+   所有依赖项将在 $GOPATH/src 目录中下载，而没有版本控制。

当我们执行 go get 时，它将在 $GOPATH/src 目录中下载所需的包。请运行下面的 go get 命令。

```go
go get github.com/pborman/uuid
```

它将下载位于该位置的包。

```go
$GOPATH/src/github.com/pborman/uuid
```

请注意上面的go get命令没有指定版本。因此，它下载最新版本。此外，请注意下载的包。即使它没有列出任何版本信息。这是个问题。如果**[github.com](http://github.com)/pborman/uuid**包有更新，而你想获取该更新，由于没有版本控制，更新的包将被下载到同一位置，替换掉旧的包。

## **在Go版本1.11中**

在Go 1.11中，模块被引入但尚未定型。因此，如果你仍在使用它，最好切换到最新版本。

## **在Go版本1.13之后**

我们已经讨论了在预模块时代存在的所有问题。现在让我们看看这些问题是如何通过引入模块得到解决的。

第一个问题是。

+   所有Go项目在$GOPATH/src目录下。

有了模块，这不再是一个要求。

+   没有本地依赖管理支持。

模块在Go中引入了本地依赖管理。通过模块，它提供了两个新的文件。

1.  go.mod

1.  go.sum

通过**go.mod**和**go.sum**文件，我们能够安装特定版本的依赖项而不会破坏任何内容。我们在本教程开始时已经简要介绍了这些文件。稍后在教程中，我们将详细讨论。

+   所有依赖项将被下载到**$GOPATH/pkg/mod**目录，并带有版本信息。

因此，如果你下载同一库的不同版本，则这两个版本将下载到**$GOPATH/pkg/mod**中的不同目录，而不会相互覆盖。**$GOPATH/pkg/mod**中将包含两个内容。

1.  缓存——这是所有依赖项将下载到的文件夹，以及压缩代码。

1.  所有下载的依赖项的压缩代码将从缓存目录复制过来。

现在让我们创建一个模块。我们讨论的内容将在那时更加清晰。

# **创建模块**

可以使用以下命令创建一个模块。

```go
go mod init {module_import_path}
```

让我们创建一个模块。

```go
go mod init learn
```

此命令将在同一目录中创建一个**go.mod**文件。现在什么是**go.mod**文件。

让我们检查一下这个文件的内容。执行`cat go.mod`。

```go
module learn

go 1.14
```

它包含模块的导入路径和创建该模块时使用的Go版本。

由于这是一个空模块，因此尚未指定任何直接依赖项。让我们在同一目录下创建一个名为**uuid.go**的文件，内容如下。

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

请注意，我们在uuid.go中也导入了依赖项。

```go
"github.com/pborman/uuid"
```

让我们运行以下命令。

```go
go mod tidy
```

此命令将下载您源文件中所需的所有依赖项，并用该依赖项更新**go.mod**文件。在运行此命令后，让我们再次检查**go.mod**文件的内容。执行`cat go.mod`。

```go
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

它列出了在uuid文件中指定的直接依赖项及其确切版本。现在让我们检查一下**go.sum**文件。

执行`cat go.sum`。

```go
github.com/google/uuid v1.0.0 h1:b4Gk+7WdP/d3HZH8EJsZpvV7EtDOgaZLtnaNGIu1adA=
github.com/google/uuid v1.0.0/go.mod h1:TIyPZe4MgqvfeYDBFedMoGGpEw/LqOeaOT+nhxU+yHo=
github.com/pborman/uuid v1.2.1 h1:+ZZIw58t/ozdjRaXh/3awHfmWRbzYxJoAdNJxe/3pvw=
github.com/pborman/uuid v1.2.1/go.mod h1:X/NO0urCmaxf9VXbdlT7C2Yzkj2IKimNn4k+gtPdI/k=
```

**go.sum** 文件列出了模块所需的直接和间接依赖项的校验和。 [github.com](http://github.com)/google/uuid 在内部被 [github.com](http://github.com)/pborman/uuid 使用。它是模块的间接依赖，因此记录在 **go.sum** 文件中。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
