<!--yml

类别：未分类

日期：2024-10-13 06:29:41

-->

# 在 Go（Golang）中向项目或模块添加依赖。

> 来源：[`golangbyexample.com/add-dependency-module-golang/`](https://golangbyexample.com/add-dependency-module-golang/)

目录。

**   概述

+   直接添加到 go.mod 文件

+   执行 go get

+   将依赖添加到源代码并执行 go mod tidy

# **概述**

模块是 Go 支持的依赖管理。模块的定义是一个相关包的集合，根目录下有 **go.mod** 文件。**go.mod** 文件定义了

+   模块导入路径。

+   模块的依赖要求以成功构建。它定义了模块的依赖需求并将其锁定到正确的版本。

模块的依赖可以分为两种。

+   直接依赖（Direct -A direct dependency）是模块直接导入的依赖。

+   间接依赖（Indirect）是由模块的直接依赖导入的依赖。此外，**go.mod** 文件中提到但在模块的任何源文件中未导入的依赖也视为间接依赖。

让我们探索几种将依赖项添加到项目的方法。

+   直接添加到 **go.mod** 文件

+   执行 **go get**

+   将依赖添加到源代码并执行 **go mod tidy**

在查看每种方式之前，我们先创建一个模块。

```go
go mod init learn
```

此命令将在同一目录下创建 **go.mod** 文件。由于这是一个空模块，因此尚未指定任何直接依赖。现在让我们探索不同的添加依赖方式。

# **直接添加到 go.mod 文件**

我们也可以直接将依赖添加到 go.mod 文件中。让我们来做这个。在 **go.mod** 文件中添加以下依赖。

```go
require github.com/pborman/uuid v1.2.1
```

有了这个依赖项，**go.mod** 文件看起来如下。

```go
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

现在我们需要下载新添加的依赖项。我们可以使用以下命令。

```go
go mod download
```

此命令将下载 **github.com/pborman/uuid** 模块及其所有依赖。此外，它还会更新 **go.sum** 文件，包含所有直接和间接依赖的校验和和版本。go build 和 go install 也会下载依赖并构建二进制文件。go run 也会下载并运行二进制文件。go mod download 命令用于在不构建或运行的情况下预下载依赖。

# **执行 go get**

只需执行 go get 也会将依赖添加到 go.mod 文件中。将我们之前添加的 uuid 依赖从 go.mod 文件中删除，并清理 go.sum 文件。现在运行以下命令。

```go
export GO111MODULE=on
go get github.com/pborman/uuid
```

现在检查 go.mod 文件的内容。执行 **cat go.mod**

```go
module learn

go 1.14

require github.com/pborman/uuid v1.2.1 //indirect
```

该依赖项将被标记为**//indirect**，因为它没有在任何源文件中使用。使用这个依赖项后，如果你执行`go build`，**//indirect**将会被 Go 自动移除。同时，它会更新**go.sum**文件，包含所有直接和间接依赖项的校验和与版本。

# **将依赖项添加到你的源代码中并执行`go mod tidy`**。

我们在上面的例子中已经看到过这个方法。基本上，`go mod tidy`命令确保你的 go.mod 文件反映出你在项目中实际使用的依赖项。当我们运行`go mod tidy`命令时，它会做两件事情。

+   添加任何在源文件中导入的依赖项。

+   移除任何在**go.mod**文件中提到但没有在任何源文件中导入的依赖项。

让我们看一个例子。移除我们之前在 go.mod 文件中添加的依赖项。你的 go.mod 文件应该如下所示。

```go
module learn

go 1.14
```

另外，让我们在同一目录下创建一个名为**uuid.go**的文件，内容如下。

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

请注意，我们在 uuid.go 中也导入了该依赖项。

```go
"github.com/pborman/uuid"
```

让我们运行下面的命令。

```go
go mod tidy
```

该命令将下载在你的源文件中所需的所有依赖项，并用该依赖项更新**go.mod**文件。运行此命令后，让我们再次检查**go.mod**文件的内容。

执行`cat go.mod`。

```go
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

它列出了在 uuid 文件中指定的直接依赖项，以及该依赖项的确切版本。现在让我们检查一下**go.sum**文件。执行`cat go.sum`。

```go
github.com/google/uuid v1.0.0 h1:b4Gk+7WdP/d3HZH8EJsZpvV7EtDOgaZLtnaNGIu1adA=
github.com/google/uuid v1.0.0/go.mod h1:TIyPZe4MgqvfeYDBFedMoGGpEw/LqOeaOT+nhxU+yHo=
github.com/pborman/uuid v1.2.1 h1:+ZZIw58t/ozdjRaXh/3awHfmWRbzYxJoAdNJxe/3pvw=
github.com/pborman/uuid v1.2.1/go.mod h1:X/NO0urCmaxf9VXbdlT7C2Yzkj2IKimNn4k+gtPdI/k=
```

现在让我们看一个例子，如果依赖项在源文件中不需要，**go mod tidy**将从**go.mod**文件中移除该依赖项。为了说明这一点，让我们移除之前创建的**uuid.go**文件。现在运行该命令。

```go
go mod tidy -v
```

它将输出如下。

```go
unused github.com/pborman/uuid
```

现在检查一下**go.mod**文件的内容。它将如下所示。

```go
module learn

go 1.14
```

该

```go
require github.com/pborman/uuid v1.2.1
```

行将被移除，因为在任何源文件中都不需要它。此外，所有**[github.com](http://github.com)/pborman/uuid**及其依赖项的条目也将从**go.sum**文件中删除。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
