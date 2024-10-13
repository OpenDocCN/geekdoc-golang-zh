<!--yml

分类：未分类

日期：2024-10-13 06:30:31

-->

# Go (Golang) 中的包与模块 – 第二部分

> 来源：[`golangbyexample.com/packages-modules-go-second/`](https://golangbyexample.com/packages-modules-go-second/)

这是 Go 语言综合教程系列的第五章。有关该系列其他章节的信息，请参考此链接 – [Golang 综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程** – [变量](https://golangbyexample.com/variables-in-golang-complete-guide/)

**上一个教程** – [包和模块 – 第一部分](https://golangbyexample.com/packages-modules-go-first)

现在让我们查看当前的教程。下面是当前教程的目录。

目录

**   概述

+   模块的类型

+   包与模块

+   向你的项目添加依赖

    +   直接添加到 go.mod 文件中

    +   执行 go get

    +   将依赖添加到你的源代码并执行 go mod tidy

+   添加供应商目录

+   模块导入路径

+   在同一模块中导入包

+   从不同模块本地导入包

+   选择库的版本

    +   在次要或补丁版本中不同

    +   在主版本中不同

+   go mod 命令

+   go.mod 文件中的直接与间接依赖

+   结论*  *# **概述**

在上一个教程中，我们详细了解了包以及模块的概述。

在本教程中，我们将重点关注模块。

# **模块的类型**

我们了解到模块是一个包含嵌套 Go 包的目录。因此，模块本质上可以视为一个包，只是它包含嵌套的包。我们在包的教程中看到，包可以是可执行包或实用包（非可执行）。类似于包，模块也可以分为两种类型。

+   **可执行模块**——我们已经知道**main**是 GoLang 中的可执行包。因此，包含 main 包的模块是可执行模块。**main**包将包含一个**main**函数，表示程序的起始。在安装包含**main**包的模块时，它将在$GOBIN 目录中创建一个可执行文件。

+   **非可执行模块或实用模块**——除了**main**包以外的任何包都是非可执行包。它不是自执行的。它仅包含实用函数和其他可被可执行包使用的实用内容。因此，如果模块不包含**main**包，那么它就是一个非可执行或实用模块。此模块旨在作为实用工具使用，将被其他模块导入。

创建模块的可执行文件（仅适用于具有主包的模块）。

+   执行`go build`，它将在当前目录中创建可执行文件。

+   运行`go install`，它将在$GOBIN 目录中创建可执行文件。

# **包与模块**。

根据模块定义，它是一个包含嵌套和相关 Go 包的目录，根目录下有**go.mod**文件。**go.mod**文件定义了。

+   模块导入路径。

+   模块成功构建的依赖要求。它定义了项目的依赖需求，并将其锁定到正确的版本。

模块提供了。

+   依赖管理。

+   使用模块时，Go 项目不一定要位于**$GOPATH/src**文件夹。

除了**go.mod**文件，Go 还保持一个**go.sum**文件，其中包含所有项目依赖模块的加密哈希。这是为了验证项目的依赖模块没有被更改。

模块内包的行为与之前相同。因此，任何适用于包的内容现在也适用。在这一点上没有变化。然而，当需要单独版本化时，多个包的集合可以称为模块。此外，当它是共享代码的一部分时，您希望在多个项目中共享该代码。

# **向项目添加依赖**。

让我们探索一些将依赖添加到项目的方法。

+   直接添加到**go.mod**文件。

+   执行**go get**。

+   将依赖添加到源代码并执行**go mod tidy**。

在查看每种方式之前，先创建一个模块。

```go
go mod init sample.com/learn
```

## **直接添加到 go.mod 文件**。

我们也可以直接将依赖添加到 go.mod 文件。让我们来做这个。

将以下依赖添加到**go.mod**文件中。

```go
require github.com/pborman/uuid v1.2.1
```

添加此依赖后，go.mod 文件将如下所示。

```go
module sample.com/learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

现在我们也需要下载新添加的依赖。为此，我们可以使用下面的命令。

```go
go mod download
```

这个命令将下载**github.com/pborman/uuid**模块及其所有依赖项。此外，它还会更新**go.sum**文件，包含所有直接和间接依赖项的校验和和版本。`go build`和`go install`也会下载依赖项并构建二进制文件。`go run`也会下载并运行二进制文件。`go mod download`命令用于在不构建或运行的情况下预下载依赖项。

## **执行`go get`**

只需执行`go get`也会在`go.mod`文件中添加依赖项。将我们之前在`go.mod`中添加的 uuid 依赖项删除，并清理`go.sum`文件。现在运行下面的命令。

```go
export GO111MODULE=on
go get github.com/pborman/uuid
```

现在检查`go.mod`文件的内容。执行`cat **go.mod**`。

```go
module sample.com/learn

go 1.14

require github.com/pborman/uuid v1.2.1 //indirect
```

该依赖项将标记为**//indirect**，因为它在任何源文件中都未被使用。一旦你在源文件中使用后执行`go build`，`//indirect`将会被 go 自动移除。同时，它还会更新**go.sum**文件，包含所有直接和间接依赖项的校验和和版本。

## **将依赖项添加到你的源代码中，并执行`go mod tidy`**

基本上，`go mod tidy`命令确保你的`go.mod`文件反映了你在项目中实际使用的依赖项。当我们运行`go mod tidy`命令时，它会做两件事情。

+   添加在源文件中导入的任何依赖项。

+   移除**go.mod**文件中提到但未在任何源文件中导入的依赖项。

让我们看一个例子。创建一个导入路径为“**sample.com/learn**”的模块。

```go
go mod init sample.com/learn
```

让我们在同一目录中创建一个名为**uuid.go**的文件，内容如下。

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

注意我们在`uuid.go`中也导入了依赖项。

```go
"github.com/pborman/uuid"
```

让我们运行下面的命令。

```go
go mod tidy
```

这个命令将下载在你的源文件中所需的所有依赖项，并使用该依赖项更新**go.mod**文件。运行此命令后，让我们再次检查**go.mod**文件的内容。执行`cat **go.mod**`。

```go
module sample.com/learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

# **添加 vendor 目录**

如果你想将依赖项放入 vendor 中，可以使用下面的命令来实现。

```go
go mod vendor
```

它将在你的项目目录中创建一个 vendor 目录。你也可以将 vendor 目录中的内容检查到你的版本控制系统（VCS）中。这在某种程度上变得有用，因为运行时不需要下载任何依赖项，因为它们已经存在于检查到 VCS 中的 vendor 文件夹中。

# **模块导入路径**

我们已经看到模块导入路径是用于导入该模块内所有包的前缀路径。

有三种情况决定可以使用什么导入路径名与模块。

+   该模块是一个实用模块，并且你计划发布你的模块。

+   该模块是一个实用模块，并且你不打算发布你的模块。

+   该模块是一个可执行模块。

**该模块是一个实用模块，并且你计划发布你的模块**

如果你打算发布你的模块，则模块名称应与托管该模块的代码库的 URL 匹配。Go 会尝试使用相同的导入路径从版本控制系统下载依赖项。

**该模块是一个工具模块，你不打算发布你的模块**。

当你只打算在本地使用工具模块时，就是这种情况。在这种情况下，导入路径可以是任何东西。

**该模块是一个可执行模块**。

在这种情况下，模块导入路径也可以是任何东西。即使你打算将模块提交到版本控制系统，模块导入路径也可以是非 URL，因为它不会被其他模块使用。

然而，在创建模块时使用有意义的导入路径是一个好习惯。

# **在同一模块中导入包**。

同一模块中的任何包都可以使用模块 + 包所在目录的导入路径进行导入。为了说明这一点，让我们创建一个模块。

+   创建一个**learn**目录。

+   创建一个导入路径为**“sample.com/learn”**的模块。

```go
go mod init sample.com/learn
```

+   现在创建 main.go（包含 main 包和 main 函数）。

+   以及 math/math.go – 数学包。

**main.go**。

```go
package main

import (
	"fmt"
	"sample.com/learn/math"
)

func main() {
	fmt.Println(math.Add(1, 2))
}
```

**math/math.go**。

```go
package math

func Add(a, b int) int {
    return a + b
}
```

查看我们如何在 main.go 文件中导入数学包。

```go
"sample.comlearn/math"
```

这里的导入路径是模块的导入路径，即**sample.com/learn** + 包所在的目录，即**math**。因此是“**sample.com/learn/math**”。嵌套目录中的包也可以以相同的方式导入。它的工作原理是，由于前缀是模块导入路径，因此 Go 会知道你正在尝试从同一模块导入。因此，它将直接引用，而不是下载。

# **从不同模块本地导入包**。

有时我们希望导入一个本地存在的模块。让我们了解如何导入这样的模块。但首先，我们必须创建一个可以被其他模块使用的模块，然后将其导入到其他模块中。为此，让我们创建两个模块。

+   **sample.com/math**模块。

+   **school**模块。

**school**模块将调用**sample.com/math**模块的代码。

让我们首先创建将被**school**模块使用的**sample.com/math**模块。

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

现在让我们创建学校模块。

+   现在在与**math**目录并排的相同路径中创建一个**school**目录。

+   创建一个模块名称为**school**。

```go
go mod init school
```

+   现在，让我们修改**go.mod**文件，以便在学校模块中导入数学模块。要导入一个未推送到版本控制系统的本地模块，我们将使用替换目录。替换目录将用你指定的路径替换模块路径。

```go
module school

go 1.14

replace sample.com/math => ../math
```

+   创建一个名为 school.go 的文件，它将使用**[sample.com](http://sample.com)/math**模块中的 Add 函数。

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

它能够调用**[sample.com](http://sample.com)/math**模块的 Add 函数，并正确输出为 6。

另外，它将更新**[sample.com](http://sample.com)/math**模块的 go.mod 版本信息。

```go
module school

go 1.14

replace sample.com/math => ../math

require sample.com/math v0.0.0-00010101000000-000000000000
```

# **选择库的版本**。

为了理解 GO 在选择在**go.mod**文件中指定的两个版本的库版本时的做法，我们必须先理解语义版本控制。

语义版本控制由三个部分组成，由点分隔。以下是版本控制的格式。

```go
v{major_version}.{minor_version}.{patch_version}
```

其中

+   **v** – 它只是一个指示版本的标志。

+   major_version – 它表示库中不兼容的 API 变化。因此，当库中有不向后兼容的变化时，major_version 会递增。

+   minor_version – 它表示库功能的向后兼容变化。因此，当库中有某些功能变化，但这些变化是向后兼容的情况下，minor 版本会递增。

+   patch_version – 它表示库中以向后兼容的方式修复的错误。因此，当库的现有功能修复了错误时，在这种情况下，patch_version 会递增。

现在可以有两种情况。

+   使用了两个相同库的版本，它们只在小版本和补丁版本上有所不同。它们的主要版本是相同的。

+   使用了两个相同库的版本，它们在主要版本上有所不同。

让我们看看 go 在上述两种情况下遵循什么方法。

## **在小版本或补丁版本上有所不同**。

Go 在选择两个版本在**go.mod**文件中指定的库版本时遵循最小版本策略方法，这两个版本只在小版本或补丁版本上有所不同。

例如，如果你使用相同库的两个版本，它们是。

```go
1.2.0
```

并且

```go
1.3.0
```

然后 go 将选择 1.3.0，因为它是最新版本。

## **在主要版本上有所不同**。

Go 将主要版本视为不同的模块。那这意味着什么呢？这本质上意味着导入路径将以主要版本作为后缀。让我们以任何 go 库的 VCS 为**github.com/sample**为例。让我们最新的语义版本是。

```go
v8.2.3
```

然后 go.mod 文件将如下所示。

```go
module github.com/sample/v8

go 1.13

..
```

它在导入路径中有主要版本。因此，任何使用此示例库的库必须像这样导入。

```go
import "github.com/sample/v8"
```

如果未来发布**v9**版本，则必须像这样导入到应用程序中。

```go
import "github.com/sample/v9"
```

此外，该库将更改其 go.mod 文件以反映 v9 主要版本。

```go
module github.com/sample/v9
```

它本质上允许在同一 go 应用程序中使用相同库的不同主要版本。当在同一应用程序中导入相同库的不同主要版本时，我们还可以给出有意义的名称。例如。

```go
import sample_v8 "github.com/sample/v8"
import sample_v9 "github.com/sample/v9"
```

这也被称为**语义导入版本控制**。

还要注意

+   对于第一个版本，不在 go.mod 文件中指定版本是可以的。

+   导入相同库的不同主要版本时要小心。注意可能与新版本一起提供的新功能。

同样出于同样的原因，当你使用更新特定模块时。

```go
go get -u
```

然后它只会升级到最新的小版本或补丁版本（如果适用）。例如，假设应用程序使用的当前版本是

```go
v1.1.3
```

同样，假设我们有以下可用版本。

```go
v1.2.0
v2.1.0
```

然后当我们运行时

```go
go get
```

然后它将更新为

```go
v1.2.0
```

原因是因为 go get 只会更新小版本或补丁版本，但从不更新主要版本，因为 go 将模块的主要版本视为完全不同的模块。

要升级主要版本，请在 **go.mod** 文件中显式指定升级的依赖项或执行该版本的 go get。

还有几点关于升级模块的注意事项。

+   要仅将依赖项升级到其最新补丁版本，请使用以下命令。

```go
go get -u=patch <dependency_name></dependency_name>
```

+   要将依赖项升级到特定版本，请使用以下命令。

```go
go get dependency@version
```

+   要将依赖项升级到特定提交，请使用以下命令。

```go
go get <dependency_name>@commit_number</dependency_name>
```

+   要将所有依赖项升级到它们最新的小版本和补丁版本，请使用以下命令。

```go
go get ./...
```

# **go mod 命令**

以下是 go mod 命令的一些选项。

+   **下载 –** 它将把所需的依赖项下载到 $GOPATH/pkg/mod/cache 文件夹中。同时，它将用所有直接和间接依赖项的校验和和版本更新 **go.sum** 文件。

+   **编辑** – 这是用于编辑 go.mod 文件。它提供了一组编辑标志。运行以下命令查看所有可用的编辑标志。

```go
go help mod edit
```

go help mod edit 例如，以下是一些可用的编辑标志。

1.  **-fmt** 标志将格式化 go.mod 文件。它不会进行其他更改。

1.  **-module** 标志可用于设置模块的导入路径。

+   **图形** – 这可以用来打印模块需求依赖图。

+   **初始化** – 我们已经在上面看到这个命令的用法。它用于初始化一个新模块。

+   **整理** – 此命令将下载源文件中所需的所有依赖项。

+   **供应商** – 如果你想为你的依赖项创建供应商，则可以使用以下命令实现相同的功能。它将在你的项目目录中创建一个 vendor 目录。你也可以将 vendor 目录检查到你的版本控制系统（VCS）中。

+   **验证 –** 此命令检查当前下载的依赖项是否已被修改。如果任何下载的依赖项经过验证，它将以非零代码退出。

+   **为什么 –** 此命令分析来自主模块的包图。它打印从主模块到给定包的最短路径。例如，在“从不同模块本地导入包”部分中创建的 school 模块，如果我们打印为什么命令如下。

```go
go mod why sample.com/math
```

然后以下将是输出。

```go
# sample.com/math
school
sample.com/math
```

输出说明 [sample.com](http://sample.com)/math 包在图中距离主模块（在这里是 school）有一段距离。

# **go.mod 文件中的直接与间接依赖**

直接依赖是模块直接导入的依赖。间接依赖是由模块的直接依赖导入的依赖。此外，任何在 **go.mod** 文件中提到但未在模块的任何源文件中导入的依赖也被视为间接依赖。

**go.mod** 文件仅记录直接依赖。然而，在以下情况下，它可能会记录间接依赖。

+   任何在你直接依赖的 go.mod 文件中未列出的间接依赖，或者如果直接依赖没有 go.mod 文件，则该直接依赖将以 //direct 作为后缀添加到 go.mod 文件中。

+   任何未在模块的任何源文件中导入的依赖（我们之前在教程中已经看到过这个示例）。

**go.sum** 将记录直接和间接依赖的校验和。

**go.mod 文件中的间接依赖示例**

让我们通过一个示例来理解它。为此，让我们先创建一个模块。

```go
git mod init sample.com/learn
```

让我们在 go.mod 文件中添加 colly 库版本 v1.2.0 作为依赖。colly 版本 v1.2.0 没有 go.mod 文件。

```go
module sample.com/learn

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

现在执行 go build。由于 colly 版本 v1.2.0 没有 go.mod 文件，colly 所需的所有依赖将以 //indirect 作为后缀添加到 go.mod 文件中。执行 go build。现在检查 go.mod 文件。你会看到以下文件内容。

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

所有其他依赖都以 **//indirect** 作为后缀。还要检查所有直接和间接依赖将在 go.sum 文件中记录。

# **结论**

这就是 golang 中的包和模块。希望你喜欢这篇文章。请在评论中分享反馈/错误/改进建议。

****下一个教程** – **[变量](https://golangbyexample.com/variables-in-golang-complete-guide/)****上一个教程** – **[包和模块 – 第一部分](https://golangbyexample.com/packages-modules-go-first)

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/) *
