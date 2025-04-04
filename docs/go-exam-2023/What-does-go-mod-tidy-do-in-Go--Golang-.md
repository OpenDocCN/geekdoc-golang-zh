<!--yml

类别：未分类

日期：2024-10-13 06:29:36

-->

# 在 Go (Golang)中，go mod tidy 的作用是什么

> 来源：[`golangbyexample.com/go-mod-tidy/`](https://golangbyexample.com/go-mod-tidy/)

目录

+   概述

+   示例

# **概述**

此命令将基本上将**go.mod**文件与源文件中所需的依赖项进行匹配。

+   下载源文件中所需的所有依赖项，并用该依赖项更新**go.mod**文件。

+   移除**go.mod**文件中在源文件中不需要的所有依赖项。

下面是该命令的使用格式

```go
go mod tidy [-v]
```

使用**-v**标志，go mod tidy 将打印从**go.mod**文件中删除的所有未使用模块的信息（如果有）。

# **示例**

让我们看一个例子。创建一个导入路径为“**learn**”的模块。

```go
go mod init learn
```

此命令将在同一目录中创建一个**go.mod**文件。由于这是一个空模块，因此尚未指定任何直接依赖项。让我们在同一目录中创建一个名为**uuid.go**的文件，其内容如下

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

让我们运行以下命令

```go
go mod tidy
```

此命令将下载源文件中所需的所有依赖项，并用该依赖项更新**go.mod**文件。运行此命令后，让我们再次检查**go.mod**文件的内容。

执行**cat go.mod**

```go
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

它列出了在 uuid 文件中指定的直接依赖项，以及该依赖项的确切版本。现在让我们检查**go.sum**文件。

执行**cat go.sum**

```go
github.com/google/uuid v1.0.0 h1:b4Gk+7WdP/d3HZH8EJsZpvV7EtDOgaZLtnaNGIu1adA=
github.com/google/uuid v1.0.0/go.mod h1:TIyPZe4MgqvfeYDBFedMoGGpEw/LqOeaOT+nhxU+yHo=
github.com/pborman/uuid v1.2.1 h1:+ZZIw58t/ozdjRaXh/3awHfmWRbzYxJoAdNJxe/3pvw=
github.com/pborman/uuid v1.2.1/go.mod h1:X/NO0urCmaxf9VXbdlT7C2Yzkj2IKimNn4k+gtPdI/k=
```

现在让我们再看一个示例，如果**go mod tidy**在源文件中不需要，则会从**go.mod**文件中移除依赖项。为此，让我们删除上面创建的**uuid.go**文件。现在运行该命令

```go
go mod tidy -v
```

它将输出以下内容

```go
unused github.com/pborman/uuid
```

现在检查**go.mod**文件的内容。它将如下所示

```go
module learn

go 1.14
```

该

```go
require github.com/pborman/uuid v1.2.1
```

行将被删除，因为它在任何源文件中都不需要。此外，所有**[github.com](http://github.com)/pborman/uuid**及其依赖项的条目也将从**go.sum**文件中删除。


