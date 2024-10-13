<!--yml

分类：未分类

日期：2024-10-13 06:29:14

-->

# Go 中的模块 (Golang)

> 来源：[`golangbyexample.com/modules-golang/`](https://golangbyexample.com/modules-golang/)

目录

+   概述

+   模块世界之前

    +   在 Go 版本 1.11 之前

    +   在 Go 版本 1.11 中

    +   在 Go 版本 1.13 之后

+   创建模块

+   向你的项目添加依赖

    +   直接将其添加到 go.mod 文件

    +   执行 go get

    +   将依赖添加到你的源代码并执行 go mod tidy

+   添加供应商目录

+   模块导入路径

    +   该模块是一个实用模块，且你计划发布你的模块

    +   该模块是一个实用模块，你不打算发布你的模块

    +   该模块是一个可执行模块

+   在同一模块内导入包

+   从不同模块本地导入包

+   选择库的版本

    +   在次要或补丁版本中有所不同

    +   在主要版本中有所不同

+   go mod 命令

+   go.mod 文件中的直接与间接依赖

    +   go.mod 文件中的间接依赖示例

+   结论

# **概述**

模块是 Go 对依赖管理的支持。模块的定义是一个相关包的集合，其根目录下有 **go.mod**。**go.mod** 文件定义了

+   模块导入路径。

+   模块的依赖要求以确保成功构建。它定义了项目的依赖要求并将其锁定到正确的版本。

将模块视为包含一组包的目录。这些包也可以嵌套。模块提供了。

+   依赖管理。

+   有了模块，Go 项目不必一定位于 **$GOPATH/src** 文件夹中。

除了 **go.mod** 文件，Go 还保留了 **go.sum** 文件，其中包含所有项目依赖模块的加密哈希值。这是为了验证你的项目依赖模块没有变化。

# **模块之前的世界**

让我们逐一查看版本变化，以充分理解之前的限制以及自模块以来的变化。

+   在 Go 版本 1.11 之前，模块根本不存在。

+   Go 版本 1.11 – 模块引入但尚未最终确定。

+   Go 版本 1.13 – 引入了模块。

## **在 Go 版本 1.11 之前**

在模块之前，Go 只有包。 `$GOPATH` 位置将有三个目录。

+   src

+   pkg

+   bin

这些是在模块时代之前存在的问题。

+   所有 Go 项目在 `$GOPATH/src` 目录中。

+   没有本地依赖管理支持。

+   所有依赖项将下载到 `$GOPATH/src` 目录中而不进行版本控制。

让我们逐一看一下每个问题。

+   任何 GO 项目都必须位于 `$GOPATH/src` 目录内。

这是一个很大的限制，因为它限制了你可以放置项目的位置。

+   没有本地依赖管理支持。

此外，模块之前还有一个问题是没有办法在项目中指定依赖项。虽然有像 dep 和 glide 这样的替代解决方案，但缺乏本地解决方案。

+   所有依赖项将下载到 `$GOPATH/src` 目录中而不进行版本控制。

当我们执行 `go get` 时，它将在 `$GOPATH/src` 目录中下载所需的包。运行下面的 `go get` 命令。

```go
go get github.com/pborman/uuid
```

它将下载位于该位置的包。

```go
$GOPATH/src/github.com/pborman/uuid
```

请注意上面的 `go get` 命令没有指定版本，因此它下载了最新版本。同时注意下载的包，甚至没有列出任何版本信息。这是一个问题。如果 **[github.com](http://github.com)/pborman/uuid** 包有更新，而你想获取该更新，由于没有版本控制，更新的包将下载到同一位置，替换旧版本。

## **在 Go 版本 1.11 中**

在 Go 1.11 中，引入了模块但尚未最终确定。因此，如果你仍在使用它，最好切换到最新版本。

## **在 Go 版本 1.13 之后**

我们已经讨论了在模块之前存在的所有问题。现在让我们看看这些问题是如何通过引入模块得到解决的。

第一个问题是。

+   所有 Go 项目在 `$GOPATH/src` 目录中。

有了模块，这不再是一个要求。

+   没有本地依赖管理支持。

模块在 Go 中引入了本地依赖管理。通过模块，它提供了两个新文件。

1.  go.mod

1.  go.sum

通过 go.mod 和 go.sum 文件，我们能够安装精确版本的依赖项而不破坏任何内容。我们在本教程开头已经简要介绍了这些文件。稍后在教程中，我们将详细查看它们。

+   所有依赖项将以版本控制的形式下载到**$GOPATH/pkg/mod**目录中。

因此，如果你下载同一库的不同版本，那么两者将被下载到**$GOPATH/pkg/mod**中的不同目录，而不会相互覆盖。**$GOPATH/pkg/mod**中将有两个内容。

+   缓存 - 这是所有依赖项将下载并包含压缩代码的文件夹。

+   所有下载的依赖项的压缩代码将从缓存目录复制过来。

还有一个新环境变量被引入，名为**GO111MODULE**。

当 GO111MODULE=off 时，go get 将以旧的方式运行，将依赖项下载到$GOPATH/src 文件夹中。

当 GO111MODULE=on 时，go get 将以新方式运行，所有模块将以版本控制的形式下载到$GOPATH/pkg/mod/cache 文件夹中。

当 GO111MODULE=auto 时。

+   当在$GOPATH/src 文件夹外运行 go get 时，它将表现得像是 GO111MODULE=on。

+   当在$GOPATH/src 文件夹内运行 go get 时，它将表现得像是 GO111MODULE=off。

现在让我们创建一个模块。我们讨论的内容在那时会更加清晰。

# **创建模块**

可以使用以下命令创建模块。

```go
go mod init {module_import_path}
```

让我们再看看之前讨论过的**go.mod**和**go.sum**文件。

**go.mod**

这是模块依赖文件。它将包含三项内容。

+   模块名称在顶部。

+   创建模块时所用的 go 版本。

+   模块的直接依赖项。

**go.sum**

该文件列出了所需的直接和间接依赖项的校验和及其版本。需要说明的是，go.mod 文件足以进行成功构建。go.sum 文件中的校验和用于验证每个直接和间接依赖项的校验和。

现在问题是**import_path**是什么。**import_path**是任何其他模块用于导入你的模块的前缀路径。

转到$GOPATH/src 文件夹外的任何目录。假设目录名称为**learn**。

```go
mkdir learn
cd learn
```

假设模块名称也是**learn**。

```go
go mod init learn
```

此命令将在同一目录中创建一个**go.mod**文件。那什么是 go.mod 文件呢？

让我们检查一下这个文件的内容。

做一个**cat go.mod**。

```go
module learn

go 1.14
```

当使用 init 命令首次创建模块时，go.mod 文件只会包含两个内容。

+   模块名称在顶部。

```go
module learn
```

创建模块时所用的 go 版本。

```go
go 1.14
```

由于这是一个空模块，因此尚未指定任何直接依赖项。让我们在同一目录中创建一个名为**uuid.go**的文件，内容如下。

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

让我们运行下面的命令。

```go
go mod tidy
```

此命令将下载源文件中所需的所有依赖项，并用该依赖项更新 **go.mod** 文件。运行此命令后，让我们再次检查 **go.mod** 文件的内容。

执行 **cat go.mod**。

```go
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

它列出了在 uuid 文件中指定的直接依赖项及其确切版本。现在让我们也检查一下 **go.sum** 文件。

执行 **cat go.sum**。

```go
github.com/google/uuid v1.0.0 h1:b4Gk+7WdP/d3HZH8EJsZpvV7EtDOgaZLtnaNGIu1adA=
github.com/google/uuid v1.0.0/go.mod h1:TIyPZe4MgqvfeYDBFedMoGGpEw/LqOeaOT+nhxU+yHo=
github.com/pborman/uuid v1.2.1 h1:+ZZIw58t/ozdjRaXh/3awHfmWRbzYxJoAdNJxe/3pvw=
github.com/pborman/uuid v1.2.1/go.mod h1:X/NO0urCmaxf9VXbdlT7C2Yzkj2IKimNn4k+gtPdI/k=
```

**go.sum** 文件列出了模块所需的直接和间接依赖项的校验和。[github.com](http://github.com)/google/uuid 是由 [github.com](http://github.com)/pborman/uuid 内部使用的。它是模块的间接依赖项，因此记录在 **go.sum** 文件中。

我们现在也可以运行这个文件，它将产生正确的输出。

```go
go run uuid.go
```

**输出**

```go
e594dc4d9a754bcb83b56e89b18b4b46
```

上述方法中，我们在源文件中添加了一个依赖项，并使用 `go mod tidy` 命令下载该依赖项并将其添加到 `go.mod` 文件中。

**模块的类型**

我们了解到模块是一个包含嵌套 Go 包的目录。因此，模块本质上可以被视为一个只包含嵌套包的包。我们在包教程中看到，包可以是可执行包或实用包（非可执行）。与包类似，模块也可以有两种类型。

+   **可执行模块** – 我们已经知道 **main** 是 GoLang 中的可执行包。因此，包含主包的模块就是可执行模块。主包将包含一个主函数，表示程序的开始。安装包含 **main** 包的模块时，它将在 **$GOBIN** 目录中创建可执行文件。

+   **非可执行模块或实用模块** – 除了 **main** 包以外的任何包都是非可执行包。它不是自我可执行的。它仅包含可以被可执行包使用的实用函数和其他实用功能。因此，如果模块不包含 **main** 包，它将是一个非可执行或实用模块。该模块旨在作为实用工具使用，将被其他模块导入。

要创建一个模块的可执行文件（仅适用于包含主包的模块）。

+   进行一次 `go build`，它将在当前目录创建可执行文件。

+   执行 `go install`，它将在 **$GOBIN** 目录中创建可执行文件。

# **将依赖项添加到你的项目中**

让我们探索一些将依赖项添加到项目中的方法。

+   直接添加到 **go.mod** 文件中。

+   执行 **go get**。

+   将依赖项添加到你的源代码中，并执行 **go mod tidy**。

在查看每种方法之前，我们先创建一个模块。

```go
go mod init learn
```

## **直接添加到 go.mod 文件中**

我们也可以直接将依赖项添加到 `go.mod` 文件中。让我们来做这个。

将以下依赖项添加到 `go.mod` 文件中。

```go
require github.com/pborman/uuid v1.2.1
```

添加此依赖项后，`go.mod` 文件将如下所示。

```go
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

现在我们还需要下载新添加的依赖项。为此，我们可以使用下面的命令。

```go
go mod download
```

此命令将下载**github.com/pborman/uuid**模块及其所有依赖项。它还将更新**go.sum**文件，包含所有直接和间接依赖项的校验和和版本。`go build`以及`go install`也将下载依赖项并构建二进制文件。`go run`也会下载并运行二进制文件。`go mod download`命令在您希望预先下载依赖项而不构建或运行时使用。

## **执行`go get`**

只需执行`go get`也会将依赖项添加到 go.mod 文件中。从 go.mod 文件中删除我们之前添加的 uuid 依赖项，并清理 go.sum 文件。现在运行以下命令。

```go
export GO111MODULE=on
go get github.com/pborman/uuid
```

现在检查 go.mod 文件的内容。

执行**cat go.mod**。

```go
module learn

go 1.14

require github.com/pborman/uuid v1.2.1 //indirect
```

该依赖项将被标记为**//indirect**，因为它未在任何源文件中使用。一旦您在源文件中使用此依赖项并执行`go build`，`//indirect`将被 go 自动移除。此外，它还将更新**go.sum**文件，包含所有直接和间接依赖项的校验和和版本。

## **将依赖项添加到您的源代码并执行`go mod tidy`**

我们在上面的示例中已经看到过这种方法。基本上，`go mod tidy`命令确保您的 go.mod 文件反映了您在项目中实际使用的依赖项。当我们运行`go mod tidy`命令时，它将执行两件事情。

+   添加在源文件中导入的任何依赖项。

+   删除在**go.mod**文件中提到但未在任何源文件中导入的依赖项。

# **添加供应商目录**

如果您想要管理您的依赖项，则可以使用以下命令来实现相同的目的。

```go
go mod vendor
```

这将在您的项目目录中创建一个供应商目录。您还可以将供应商目录检查到您的版本控制系统（VCS）中。这在某种意义上非常有用，因为不需要在运行时下载任何依赖项，因为它已经存在于检查到 VCS 中的供应商文件夹中。

# **模块导入路径**

我们已经看到，模块导入路径是用于导入该模块内所有包的前缀路径。

有三种情况决定可以与模块使用的导入路径名称。

+   该模块是一个实用模块，您计划发布您的模块。

+   该模块是一个实用模块，您不打算发布您的模块。

+   该模块是一个可执行模块。

## **该模块是一个实用模块，您计划发布您的模块。**

如果您计划发布您的模块，则模块名称应与托管该模块的仓库的 URL 匹配。Go 尝试使用相同的模块导入路径从 VCS 下载依赖项。

## **该模块是一个实用模块，您不打算发布您的模块。**

这是您只打算在本地使用实用模块的情况。在这种情况下，导入路径可以是任何内容。

## **该模块是一个可执行模块。**

在这种情况下，模块导入路径可以是任何内容。即使你打算将模块提交到 VCS，模块导入路径也可以是非 URL，因为其他模块不会使用它。

不过，创建模块时使用有意义的导入路径是个好习惯。

# **在同一模块内导入包**

在同一模块内，任何包都可以通过模块的导入路径加上包含该包的目录来导入。为了说明，让我们创建一个模块。

+   创建一个**learn**目录。

+   创建一个导入路径为“learn”的模块。

```go
go mod init learn
```

+   现在创建`main.go`（包含主包和主函数）。

+   和 math/math.go – math 包。

**main.go**

```go
package main

import (
	"fmt"
	"learn/math"
)

func main() {
	fmt.Println(math.Add(1, 2))
}
```

**math/math.go**

```go
package math

func Add(a, b int) int {
    return a + b
}
```

查看我们如何在`main.go`文件中导入 math 包。

```go
"learn/math"
```

这里的导入路径是模块的导入路径，即**learn**加上包含该包的目录**math**。因此是“learn/math”。嵌套目录中的包也可以以同样的方式导入。工作原理是，前缀是模块导入路径，因此 go 会知道你试图从同一模块导入。所以它将直接引用，而不是下载。

# **从不同模块本地导入包**

有时我们想要导入一个本地存在的模块。让我们了解如何导入这样的模块。但首先，我们必须创建一个可以被其他人使用的模块，然后将其导入到另一个模块中。为此，让我们创建两个模块。

+   **sample.com/math**模块

+   **school**模块

**school**模块将调用**sample.com/math**模块的代码。

让我们先创建**sample.com/math**模块，该模块将被**school**模块使用。

+   创建一个**math**目录。

+   创建一个导入路径为**sample.com/math**的模块。

```go
go mod init sample.com/math
```

+   在**math**目录中创建一个名为**math.go**的文件，内容如下。

```go
package math

func Add(a, b int) int {
	return a + b
}
```

现在让我们创建 school 模块。

+   现在在与**math**目录并排的相同路径下创建一个**school**目录。

+   创建一个名为**school**的模块。

```go
go mod init school
```

+   现在让我们修改**go.mod**文件，以在 school 模块中导入 math 模块。要导入一个未推送到 VCS 的本地模块，我们将使用替换目录。替换目录将用你指定的路径替换模块路径。

```go
module school

go 1.14

replace sample.com/math => ../math
```

+   创建文件`school.go`，该文件将使用**[sample.com](http://sample.com)/math**模块中的 Add 函数。

```go
package main

import (
	"fmt"
	"sample.com/math"
)

func main() {
	fmt.Println(math.Add(2, 4))
}
```

现在运行`go run`。

```go
go run school.go
```

它能够调用**[sample.com](http://sample.com)/math**模块的 Add 函数，并正确输出 6。

它还将使用**[sample.com](http://sample.com)/math**模块的版本信息更新 go.mod。

```go
module school

go 1.14

replace sample.com/math => ../math

require sample.com/math v0.0.0-00010101000000-000000000000
```

# **选择库的版本**

要了解 GO 在选择**go.mod**文件中指定的两个版本的库时的方式，我们首先需要理解语义版本控制。语义版本控制由用点分隔的三个部分组成。以下是版本控制的格式。

```go
v{major_version}.{minor_version}.{patch_version}
```

其中

+   **v** – 只是指示它是一个版本。

+   major_version – 它表示库中的不兼容 API 更改。因此，当库中发生不向后兼容的更改时，major_version 将会递增。

+   minor_version – 它表示库在向后兼容的方式下功能的变化。因此，当库中有一些功能变化，但这些变化是向后兼容的，那么在这种情况下，次要版本将递增。

+   patch_version – 它表示库中的错误修复以向后兼容的方式进行。因此，当现有库功能存在错误修复时，在这种情况下，patch_version 将递增。

现在可能有两种情况

+   使用的两个相同库的版本只在次要和补丁版本上不同。它们的主要版本是相同的。

+   使用了两个主要版本不同的相同库。

让我们看看 Go 在上述两种情况下遵循什么方法。

## **次要或补丁版本不同**

Go 在选择`go.mod`文件中指定的两个仅在次要或补丁版本上不同的库版本时，遵循最低版本策略方法。

例如，在使用相同库的两个版本时，可能是

```go
1.2.0
```

和

```go
1.3.0
```

然后 Go 将选择 1.3.0，因为它是最新版本。

## **主要版本不同**

Go 将主要版本视为一个不同的模块。那么，这意味着什么？这基本上意味着导入路径将以主要版本作为后缀。让我们以 VCS 为**github.com/sample**的任意 Go 库为例。让我们最新的语义版本是

```go
v8.2.3
```

然后`go.mod`文件将如下所示

```go
module github.com/sample/v8

go 1.13

..
```

它在导入路径中具有主要版本。因此，任何使用此示例库的库都必须像这样导入它。

```go
import "github.com/sample/v8"
```

如果将来发布**v9**版本，则必须以如下方式在应用程序中导入它。

```go
import "github.com/sample/v9"
```

此外，go-redis 库将更改其`go.mod`文件以反映 v9 主要版本。

```go
module github.com/samples/v9
```

它基本上允许在同一 Go 应用程序中使用相同库的不同主要版本。当在同一应用程序中导入同一库的不同主要版本时，我们还可以给出有意义的名称。例如

```go
import redis_v8 "github.com/sample/v8"
import redis_v9 "github.com/sample/v9"
```

这也被称为**语义导入版本控制**。另外请注意

+   对于第一个版本，在`go.mod`文件中不指定版本是可以的。

+   此外，在导入同一库的不同主要版本时要小心。注意新版本中可能提供的新功能。

也因为同样的原因，当你使用更新特定模块时

```go
go get -u
```

然后它只会升级到适用的最新次要版本或补丁版本。例如，假设当前应用程序使用的版本是

```go
v1.1.3
```

此外，假设我们有以下可用版本

```go
v1.2.0
v2.1.0
```

然后当我们运行

```go
go get
```

然后它会更新到

```go
v1.2.0
```

原因是因为`go get`只会更新次要版本或补丁版本，而不会更新主要版本，因为 Go 将模块的主要版本视为完全不同的模块。

要升级主要版本，请在**go.mod**文件中显式指定升级的依赖，或者执行该版本的 go get。

还有一些关于升级模块的注意事项。

+   要将依赖升级到其最新的补丁版本，请使用以下命令。

```go
go get -u=patch <dependency_name></dependency_name>
```

+   要将依赖升级到特定版本，请使用以下命令。

```go
go get dependency@version
```

+   要将依赖升级到特定提交，请使用以下命令。

```go
go get <dependency_name>@commit_number</dependency_name>
```

+   要将所有依赖升级到其最新的小版本和补丁版本，请使用以下命令。

```go
go get ./...
```

# **go mod 命令**

下面是 go mod 命令的一些选项。

+   **download –** 它将下载所需的依赖到$GOPATH/pkg/mod/cache 文件夹。此外，它将更新 go.sum 文件，包含所有直接和间接依赖的校验和及版本。

+   **edit –** 该命令用于编辑 go.mod 文件。它提供了一组编辑标志。运行以下命令查看所有可用的编辑标志。

```go
go help mod edit
```

例如，下面是一些可用的编辑标志。

1.  **-fmt**标志将格式化 go.mod 文件，但不会做其他更改。

1.  **-module**标志可用于设置模块的导入路径。

+   **graph –** 该命令可用于打印模块需求依赖图。

+   **init –** 我们已经看到过该命令的使用。它用于初始化一个新模块。

+   **tidy –** 此命令将下载源文件中所需的所有依赖。

+   **vendor –** 如果您想对依赖进行供应管理，则可以使用以下命令来实现。它将在项目目录内创建一个 vendor 目录。您还可以将 vendor 目录中的内容检查到您的版本控制系统（VCS）中。

+   **verify –** 此命令检查当前下载的依赖是否被修改。如果任何已下载的依赖被验证，程序将以非零代码退出。

+   **why –** 此命令分析主模块的包图。它打印从主模块到给定包的最短路径。例如，在“从不同模块本地导入包”部分创建的学校模块，如果我们像下面这样打印 why 命令。

```go
go mod why sample.com/math
```

然后，下面将是输出。

```go
# sample.com/math
school
sample.com/math
```

输出显示[`sample.com/math`](http://sample.com)/math 包在图中距离主模块（这里是学校）为一。

# **go.mod 文件中的直接与间接依赖**

直接依赖是模块直接导入的依赖。间接依赖是由模块的直接依赖导入的依赖。此外，任何在**go.mod**文件中提到但未在模块的任何源文件中导入的依赖也视为间接依赖。

**go.mod**文件仅记录直接依赖。然而，在以下情况下，它可能会记录间接依赖。

+   任何不在直接依赖的 go.mod 文件中列出的间接依赖，或者如果直接依赖没有 go.mod 文件，那么该直接依赖将被添加到 go.mod 文件中，后缀为//direct。

+   任何未在模块的任何源文件中导入的依赖项（我们在教程中已经看到过这个例子）。

**go.sum**将记录直接和间接依赖项的校验和。

## **go.mod 文件中的间接依赖项示例**

让我们通过一个例子来理解这一点。为此，让我们首先创建一个模块。

```go
git mod init learn
```

让我们在 go.mod 文件中添加 colly 库版本 v1.2.0 作为依赖项。colly 版本 v1.2.0 没有 go.mod 文件。

```go
module learn

go 1.14

require github.com/gocolly/colly v1.2.0
```

现在创建一个文件 learn.go。

```go
package main

import (
	"github.com/gocolly/colly"
)

func main() {
	_ = colly.NewCollector()
}
```

现在进行 go build。由于 colly 版本 v1.2.0 没有 go.mod 文件，colly 所需的所有依赖项将以//indirect 为后缀添加到 go.mod 文件中。

进行 go build。现在检查 go.mod 文件。你会看到文件的以下内容。

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

所有其他依赖项后缀为**//indirect**。还要检查所有直接和间接依赖项是否会记录在 go.sum 文件中。

# **结论**

这就是关于 golang 中的模块的全部内容。希望你喜欢这篇文章。请在评论中分享反馈。


