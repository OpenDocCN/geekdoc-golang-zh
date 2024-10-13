<!--yml

类别：未分类

日期：2024-10-13 06:30:26

-->

# Go（Golang）中的包和模块 – 第一部分

> 来源：[`golangbyexample.com/packages-modules-go-first/`](https://golangbyexample.com/packages-modules-go-first/)

这是 golang 综合教程系列的第四章。有关该系列其他章节，请参考此链接 – [Golang 综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程** – [包和模块 – 第二部分](https://golangbyexample.com/packages-modules-go-second/)

**上一个教程** – [设置 GO 工作区和 Hello World 程序](https://golangbyexample.com/workspace-hello-world-golang/)

现在让我们查看当前教程。以下是当前教程的目录。

目录

**   概述

+   模块

+   模块世界之前

    +   Go 版本 1.11 之前

    +   在 Go 版本 1.11 中

    +   Go 版本 1.13 之后

+   理解包及创建模块

+   模块之前创建应用程序

+   导出与未导出名称

+   嵌套包

+   导入包中的别名

+   初始化函数

+   Go 程序的执行顺序

+   导入中的空标识符

+   包命名约定

+   结论 * *# **概述**

包是 GO 中代码重用的一种方式。顾名思义，它是将相关代码分组的一种方法。Go 模块是处理 golang 中依赖关系的一种方式。

GO 应用程序文件中的每个 GO 源文件（.go 文件）都属于一个包。这就是每个**.go**文件以此开头的原因。

```go
package <package_name></package_name>
```

上述称为**包声明**。请记住这个术语，因为在本教程中将多次使用。

同一目录下的所有.go 文件都将属于同一包。关于包的一个很大误解是，包是包含.go 文件的目录的名称。这并不正确。目录只是一个目录，而包的名称就是在**包声明**中出现的名称。那么目录名称的重要性是什么呢？将在教程中逐步解释。

包可以分为两种类型。

+   **可执行包** – 只有**main**是 GoLang 中的可执行包。一个.go 文件可能属于特定目录中的主程序包。稍后我们将看到目录名称或**.go**文件名称的重要性。主程序包将包含一个主函数，标志着程序的开始。安装主程序包后，它将在**$GOBIN**目录中创建一个可执行文件。

+   **实用程序包**– 任何非主程序包都是实用程序包。它不是自执行的，仅包含可供可执行包使用的实用函数和其他实用工具。

# **模块**

模块是 Go 对依赖管理的支持。根据定义，模块是一组相关包，根目录下有**go.mod**文件。**go.mod**文件定义了

+   模块导入路径。

+   模块的依赖要求，以便成功构建。它定义了项目的依赖要求，并将其锁定到正确的版本。

将模块视为包含一组包的目录。这些包也可以是嵌套的。模块提供

+   依赖管理。

+   有了模块，Go 项目不一定必须位于**$GOPATH/src**文件夹中。

除了**go.mod**文件，Go 还保留了**go.sum**文件，包含所有项目依赖模块的加密哈希。这是为了验证项目的依赖模块未发生变化。

# **模块之前的世界**

模块在 Go 1.11 版本中引入。让我们逐版本查看变更，以便充分了解之前的限制以及自模块以来的变化。

+   Go 版本 1.11 之前 – 模块根本不存在。

+   Go 版本 1.11 – 引入了模块，但尚未最终确定。

+   Go 版本 1.13 – 引入了模块。

## **Go 版本 1.11 之前**

在模块出现之前，Go 仅有包。$GOPATH 位置将有三个目录。

+   src

+   pkg

+   bin

这些是模块时代之前存在的问题。

+   所有 Go 项目在**$GOPATH/src**目录中。

+   没有本地依赖管理支持。

+   所有依赖项将下载到**$GOPATH/src**目录中，不带版本控制。

让我们逐一查看每个问题：

+   任何 Go 项目都必须位于**$GOPATH/src**目录中。

这是一个重大限制，因为它限制了你可以放置项目的位置。

+   没有本地依赖管理支持。

此外，在模块之前的一个问题是，没有办法在项目中指定依赖项。虽然有 dep、glide 等替代解决方案，但缺少本地解决方案。

+   所有依赖项将下载到**$GOPATH/src**目录中，不带版本控制。

当我们执行 go get 时，它将在$GOPATH/src 目录中下载所需的包。因此，在 Go 1.11 版本之前，以下 go get 命令

```go
go get github.com/pborman/uuid
```

它将在该位置下载包。

```go
$GOPATH/src/github.com/pborman/uuid
```

注意上面的 go get 命令没有指定版本。因此，它下载的是最新版本。此外，请注意下载的包。它甚至没有列出任何版本信息。现在，这是一个问题。如果 **[github.com](http://github.com)/pborman/uuid** 包有更新，你想获取该更新。由于没有版本控制，更新的包将下载到相同位置，替换旧版本。

## **在 Go 版本 1.11**

在 Go 1.11 中，引入了模块，但尚未最终确定。因此，如果你仍在使用它，最好切换到最新版本。

## **在 Go 版本 1.13 之后**

我们已经讨论了在预模块时代存在的所有问题。现在让我们看看这些问题是如何通过引入模块得到解决的。

第一个问题是

+   所有 Go 项目位于 **$GOPATH/src** 目录。

有了模块，这不再是一个要求。

+   没有本地依赖管理支持。

模块在 Go 中引入了本地依赖管理。通过模块，它提供了两个新文件：

1.  go.mod

1.  go.sum

使用 **go.mod** 和 **go.sum** 文件，我们能够安装精确版本的依赖项而不破坏任何东西。我们已经在本教程开头简要介绍了这些文件。稍后在教程中，我们将详细查看它。

+   所有依赖项将在 **$GOPATH/pkg/mod** 目录中下载并进行版本控制。

所以如果你下载同一库的不同版本，那么两个版本将下载到 **$GOPATH/pkg/mod** 中的不同目录，而不会互相覆盖。**$GOPATH/pkg/mod** 将包含两样东西。

1.  **cache** – 这是所有依赖项将被下载的文件夹，连同压缩代码。

1.  所有下载的依赖项的压缩代码将从缓存目录复制过来。

此外，还引入了一个名为 **GO111MODULE** 的新环境变量。

当 **GO111MODULE=off** 时，go get 将以旧方式运行，会在 $GOPATH/src 文件夹中下载依赖项。

当 **GO111MODULE=on** 时，go get 将以新方式运行，所有模块将下载到 **$GOPATH/pkg/mod/cache** 文件夹，并进行版本控制。

当 **GO111MODULE=auto** 时，

+   在 $GOPATH/src 文件夹外运行 go get 时，它将表现得好像 GO111MODULE=on。

+   在 $GOPATH/src 文件夹中运行 go get 时，它将表现得好像 GO111MODULE=off。

现在让我们创建一个模块。我们讨论的内容将更加清晰。

# **理解包并创建模块**

可以使用以下命令来创建模块。

```go
go mod init {module_import_path}
```

让我们再次看看之前讨论过的 **go.mod** 和 **go.sum** 文件。

**go.mod**

这是模块依赖文件。它将包含三样东西

+   模块的导入路径位于顶部。

+   创建模块时所使用的 Go 版本。

+   模块的直接依赖项。

**go.sum**

此文件列出了所需的直接和间接依赖项的校验和及其版本。需要提到的是，**go.mod**文件对于成功构建来说是足够的。**go.sum**文件中的校验和用于验证每个直接和间接依赖项的校验和。

现在问题是**import_path**是什么。**import_path**是其他任何模块用来导入你的模块的前缀路径。我们将在教程的第二部分深入了解导入路径。

转到$GOPATH/src 文件夹外的任何目录。假设目录名称为**learn**。

```go
mkdir learn
cd learn
```

假设模块的导入路径是**sample.com/learn**。

```go
go mod init sample.com/learn
```

此命令将在同一目录中创建一个**go.mod**文件。

让我们检查一下该文件的内容。执行命令`cat go.mod`。

```go
module sameple.com/learn

go 1.14
```

当模块首次使用 init 命令创建时，go.mod 文件将仅包含两项内容。

+   模块的导入路径位于顶部。

```go
module sameple.com/learn
```

+   创建模块时使用的 Go 版本。

```go
go 1.14
```

由于它是一个空模块，因此尚未指定任何直接依赖项。让我们在同一目录中创建一个名为**main.go**的文件，内容如下。

**main.go**

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

注意上面文件中的包声明。

```go
package main
```

这意味着上面的源文件属于**main**包。注意我们在**main.go**中也导入了该依赖项。

```go
"github.com/pborman/uuid"
```

由于**main.go**文件属于 main 包，因此我们可以创建一个可执行文件。可执行文件始终与包含 main 包的模块的导入路径中的最后一个名称一起构建。在我们的案例中，模块的导入路径是 sample.com/learn，导入路径中的最后一个名称是 learn。因此，运行 go install 时将创建名为 learn 的可执行文件。

可以使用这三个命令中的任何一个来创建可执行文件。

+   **‘go install learn’**

+   **‘go install’**

+   **‘go install .’**

这里目录的名称无关紧要。可执行文件的名称将始终与模块名称相同。上述所有命令将在**$GOBIN**目录中创建名为**learn**的可执行文件。如果**$GOBIN**目录在你的路径中，你可以直接运行该可执行文件。

```go
learn
```

同时，go install 命令将下载源文件中所需的所有依赖项，并用该依赖项更新**go.mod**文件。

在运行该命令后，让我们再次检查**go.mod**文件的内容。

执行命令`cat go.mod`

```go
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

该文件列出了在**main.go**文件中指定的直接依赖项及其确切版本。现在让我们检查一下**go.sum**文件。

执行命令`cat go.sum`

```go
github.com/google/uuid v1.0.0 h1:b4Gk+7WdP/d3HZH8EJsZpvV7EtDOgaZLtnaNGIu1adA=
github.com/google/uuid v1.0.0/go.mod h1:TIyPZe4MgqvfeYDBFedMoGGpEw/LqOeaOT+nhxU+yHo=
github.com/pborman/uuid v1.2.1 h1:+ZZIw58t/ozdjRaXh/3awHfmWRbzYxJoAdNJxe/3pvw=
github.com/pborman/uuid v1.2.1/go.mod h1:X/NO0urCmaxf9VXbdlT7C2Yzkj2IKimNn4k+gtPdI/k=
```

**go.sum** 文件列出了模块所需的直接和间接依赖项的校验和。 [github.com](http://github.com)/google/uuid 在 [github.com](http://github.com)/pborman/uuid 中被内部使用。它是模块的间接依赖，因此记录在 **go.sum** 文件中。直接依赖和间接依赖都会下载到 $GOAPTH/pkg/mod/cache 文件夹中，并带有版本信息。

我们还可以直接运行可执行文件。

```go
learn
```

注意：**$GOBIN** 需要直接在你的路径中，以便上述命令可以运行。

**输出**

```go
e594dc4d9a754bcb83b56e89b18b4b46
```

以上方式我们在源文件中添加了一个依赖项，当我们运行 go install 时，它下载了该依赖项并将其添加到 go.mod 文件中。可执行文件也创建在 **$GOBIN** 目录中。

现在有几个要注意的点。

+   目录名称是 **learn**。

包含 **go.mod** 文件的目录名称无关紧要。你可以尝试将名称更改为任何内容。它始终会创建一个与模块导入路径最后部分相同名称的可执行文件。

+   模块的导入路径是 **sample.com/learn**。

可执行文件的名称始终是模块导入路径最后部分的名称，这里是 **learn**。导入路径的重要性将在教程的第二部分学习。现在只需理解模块导入路径用于将该模块导入到另一个模块中。如果模块导入路径仅是一个名称，那么可执行文件将只会以该名称创建。例如，模块导入路径可能仅为 **learn**。在这种情况下，可执行文件的名称也是 learn。因此，对于以下模块导入路径，可执行文件的名称将是 **learn**。

```go
sample.com/manage/learn
sample.com/learn
learn
```

+   文件名称是 **main.go**。

在运行 ‘go install’ 时，文件名称在这里并不重要。你可以尝试将文件从 main.go 更改为 test.go。之后再次运行上述命令。它将创建一个与模块名称相同的可执行文件，即 **learn**，且没有任何变化。

+   learn 模块中的包名称是 **main**。

包的名称是重要的。如果 go install 在目录中看到一个主包，它将会在 **$GOBIN** 目录中创建一个可执行文件。可执行文件的名称将与模块导入路径中的最后名称相同。

到目前为止我们学到的：

+   只有 **main** 包是可执行的。

+   包含模块的目录名称在可执行文件的名称中并不重要。

+   运行 **‘go install’** 时，它会创建一个二进制文件，名称与包含主包的 .go 文件的模块导入的最后部分相同。

+   在运行 go install 时，文件名称并不重要。你可以有任何名称。

让我们在模块之前也看看同样的例子，以了解引入模块所提供的好处。

# **在模块之前创建应用程序**。

在模块出现之前，你的所有代码都应该位于**$GOPATH/src**文件夹中。让我们在**$GOPATH/src**目录中创建一个名为**learn**的目录。

**$GOPATH/src/learn/main.go**

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

运行

```go
export GO111MODULE=off
go install
```

在模块出现之前，可执行文件始终使用包含**main**包的目录名称进行构建，而运行`go install`命令时，在我们的例子中目录的名称是**learn**。

上述所有命令将在$GOBIN 目录中创建一个名为**learn**的可执行文件。如果$GOBIN 目录在你的路径中，那么你可以直接运行这个可执行文件。

```go
learn
```

让我们看看模块之前的限制。

+   所有代码都需要位于**$GOPATH/src/**文件夹中。

+   当你执行`go install`时，它将下载**$GOPATH/src**文件夹中的直接和间接依赖项，而没有任何版本信息。你可以检查你的**$GOPATH/src**文件夹。它会下载**“github.com/pborman/uuid”**依赖项，而没有任何版本信息。

+   在代码中没有办法定义依赖版本。简单的`go get`将下载依赖的最新版本，并将其保存在**$GOPATH/src**位置，覆盖已存在的旧版本（如果有）。

模块解决了我们在第一个例子中看到的所有问题。现在我们已经理解了包和模块的基础知识，以及模块提供的优势。接下来，让我们在本教程中更详细地了解包的内容。我们将在本教程的第二部分——模块中更加关注模块。第二部分的链接是 – [`golangbyexample.com/packages-modules-go-second`](https://golangbyexample.com/packages-modules-go-second)/。

现在让我们添加另一个属于主包的文件。

**learn/subtract.go**

```go
package main
func subtract(a, b int) int {
    return a - b
}
```

**learn/main.go**

```go
package main
import "fmt"
func main() {
    fmt.Println(add(2, 1))
    fmt.Println(subtract(2, 1))
}
func add(a, b int) int {
    return a + b
}
```

请再次尝试运行`go install`命令。它将在**$GOBIN**目录中创建一个名为**learn**的可执行文件。尝试运行这个可执行文件，它将给出以下输出。此外，请注意**main.go**中的**main**函数能够调用**subtract.go**中的**subtract**函数。这是因为**main**和**subtract**都属于同一个包 main。

```go
learn $ learn
3
-1
```

所以基本上

+   在同一个包内，所有变量、函数、常量都可以在属于该包的不同.go 文件之间访问。

同一目录中的所有**.go**文件将属于同一个包。这对于所有包含包的目录都是正确的。无论该目录是否包含**go.mod**文件，这一点都不重要。让我们验证一下。将包声明更改为

```go
package subtract
```

**go.mod**

```go
module sameple.com/learn

go 1.14
```

**learn/subtract.go**

```go
package subtract

func subtract(a, b int) int {
    return a-b
}
```

**learn/main.go**

```go
package main

import "fmt"

func main() {
    fmt.Println(add(2, 1))
    fmt.Println(subtract(2, 1))
}

func add(a, b int) int {
    return a + b
}
```

注意**包声明。** 它是

```go
package subtract
```

让我们尝试运行**go install**命令。它将产生错误。

```go
can't load package: package learn: found packages subtract (subtract.go) and main (main.go) in
```

上述错误意味着目录**learn**包含两个文件**subtract.go**和**main.go**，它们分别属于不同的包，名为**subtract**和**main**。因此，在同一目录中，属于不同包的 GO 源文件是不允许的，因此出现了这个错误。将 subtract.go 中的**Package Declaration**更改为属于**main**包，错误将消失。

如你所见，我们使用了两个函数，**add**和**subtract**。这两个方法是算术函数，如果能将其作为一些公共代码的一部分，供其他部分使用，那将是很好的。这就是包的用处。我们的想法是创建一个新的包**math**，其中包含这两个函数。通过这种方式，包提供了代码重用的方法。让我们引入一个新包**math**。

**go.mod**

```go
module sameple.com/learn

go 1.14
```

**learn/math/math.go**

```go
package math

func Add(a, b int) int {
    return a + b
}
func Subtract(a, b int) int {
    return a - b
}
```

**learn/main.go**

```go
package main

import (
    "fmt"
    "sample.com/learn/math"
)

func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math.Subtract(2, 1))
}
```

如果你运行这个程序，输出将是相同的：

```go
3
1
```

关于上述程序需要注意的几点

+   **导入一个包**

main.go 文件通过**“sample.com/learn/math”**导入包，并能够使用 math.Add(..)和 math.Subtract(..)调用 Add 和 Subtract。看看我们是如何在 main.go 文件中导入 math 包的。

```go
"sample.com/learn/math"
```

这里的导入路径是模块的导入路径，即**learn** + 包所在的目录**math**。因此为**“sample.com/learn/math”**。嵌套目录中的包也可以以相同方式导入。

+   **创建一个包**

我们创建了一个新目录 math，并在其中创建了新文件 math.go。math.go 的包声明为“package math”。它还包含文件**math.go**，其中包含两个函数**Add**和**Subtract**。

首先让我们讨论导入语句。我们可以通过两种方式导入一个包。

+   导入的简写方式

```go
import (
    "fmt"
    "sample.com/learn/math"
)
```

还有另一种形式

```go
import "fmt"
import "sample.com/learn/math"
```

在程序中，你可以导入任意数量的包。GO 尝试以下方式解析。

+   检查目录$GOROOT/src – “fmt”包位于位置**$GOROOT/src/fmt**。

+   使用模块时，从模块的根开始解析路径，即包含**go.mod**文件的目录。

```go
import "sample.com/learn/math"
```

上述语句基本上意味着导入位于位置**“sample.com/learn/math”**的包。它并不意味着导入包**math**。为验证这一点，让我们将目录名称更改为**math2**。包声明仍然相同，即**“package math”**。

**go.mod**

```go
module sample.com/learn

go 1.14
```

**learn/math2/math.go –** 请注意，这里目录名称是**math2**。

```go
package math

func Add(a, b int) int {
    return a + b
}

func Subtract(a, b int) int {
    return a - b
}
```

**learn/main.go**

```go
package main

import (
    "fmt"
    "sample.com/learn/math2"
)

func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math.Subtract(2, 1))
}
```

如果你运行这个程序，输出仍然是相同的：

在**main.go**中，导入文件夹更改为**“sample.com/learn/math2”**，但**main**函数在**main.go**中仍然引用**Add**和**Subtract**作为**math.Add()**和**math.Subtract()**。所以基本上 GO 从**“math2”**目录导入**math**包。这就是我们所说的导入意味着导入位于该目录位置的包。

所以我们现在知道包目录名称的用途。

+   目录的使用是为了 GO 程序中的导入语句。在导入时，我们提供目录路径而不是包名称。GO 然后获取所有具有相同包名称的文件。

文件名怎么样？在上述情况下，文件名是**math.go**。文件名并不重要。你可以尝试将文件名从**math.go**更改为其他名称，输出仍然是相同的。

让我们继续。你可能注意到**Add**和**Subtract**都是大写的。这是为什么呢？如果它们不是大写会发生什么？如果 Add 和 Subtract 不是大写的，**main.go**将无法引用它们。这引出了我们下一个讨论话题。

# **导出名称与未导出名称**

Go 没有任何**public**、**private**或**protected**关键字。控制可见性的唯一机制是使用大写和小写格式。

+   **大写标识符**是导出的。大写字母表示这是一个导出标识符，它将在其他包中可见。

+   **未大写的标识符**不会被导出。小写表示该标识符不会被导出，只能在同一包内访问。

有五种标识符可以是导出的或未导出的。

1.  结构

1.  结构的方法

1.  结构的字段

1.  函数

1.  变量

让我们添加一个名为**multiply**的函数，它将是小写的。我们将它添加到 math 包中，看看它是否可以从**main.go**访问。

**go.mod**

```go
module sameple.com/learn

go 1.14
```

**learn/math/math.go**

```go
package math
func Add(a, b int) int {
    return a + b
}
func Subtract(a, b int) int {
    return a - b
}
func multiply(a, b int) int {
    return a * b
}
```

**learn/main.go**

```go
package main
import (
    "fmt"
    "sample.com/learn/math"
)
func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math.Subtract(2, 1))
    fmt.Println(math.multiply(2, 1))
}
```

让我们运行这个程序。它会产生错误。

```go
learn $ go install
learn $ learn
./main.go:12:14: cannot refer to unexported name math.multiply
./main.go:12:14: undefined: math.multiply
```

错误是因为**main.go**无法引用未导出的名称**math.multiply**。将 multiply 改为大写，它就能工作了。之后应该会输出如下结果。

```go
3
1
2
```

如在教程开始时提到的，**multiply**函数将在**math**包中可用。为了说明这一点，让我们在 arithmetic.go 中创建一个**Mul**函数，它将内部调用**multiply**函数。

**go.mod**

```go
module sameple.com/learn

go 1.14
```

**learn/math/math.go**

```go
package math
func Add(a, b int) int {
    return a + b
}
func Subtract(a, b int) int {
    return a - b
}
func Mul(a, b int) int {
    return multiply(a, b)
}
func multiply(a, b int) int {
    return a * b
}
```

**learn/main.go**

```go
package main
import (
    "fmt"
    "sample.com/learn/math"
)
func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math.Subtract(2, 1))
    fmt.Println(math.Mul(2, 1))
}
```

让我们运行这个程序。

```go
learn $ go install
learn $ learn
3
1
2
```

上述程序能够工作，因为在同一包内，你可以引用未导出或未大写的名称。

目前**math.go**包含**Add**和**Subtract**函数。我们可以将这两个函数分配到不同的文件中，同时仍然属于同一包**math**吗？可以。让我们在**math**目录下创建**add.go**和**subtract.go**文件。add.go 将包含**Add**函数，subtract.go 将包含**Subtract**函数。注意**add.go**和**subtract.go**的包声明都是**package math**。

**go.mod**

```go
module sameple.com/learn

go 1.14
```

**learn/math/add.go**

```go
package math
func Add(a, b int) int {
    return a + b
}
```

**learn/math/subtract.go**

```go
package math
func Subtract(a, b int) int {
    return a - b
}
```

**learn/main.go**

```go
package main
import (
    "fmt"
    "sample.com/learn/math"
)
func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math.Subtract(2, 1))
}
```

让我们运行这个程序。

```go
learn $ go install
learn $ learn
3
1
```

它能够工作，因为**main.go**中的导入语句从导入路径**“sample.com/learn/math”**中获取所有属于**math**包的标识符。

# **嵌套包**

在 GO 中，可以创建嵌套包。我们将在**math**目录中创建一个名为**advanced**的新目录。“**.” 该目录将包含**square**.go 文件，包声明为“package advanced”。

**go.mod**

```go
module sameple.com/learn

go 1.14
```

**learn/math/math.go**

```go
package math
func Add(a, b int) int {
    return a + b
}
func Subtract(a, b int) int {
    return a - b
}
```

**learn/math/advanced/advanced.go**

```go
package advanced
func Square(a int) int {
    return a * a
}
```

**learn/main.go**

```go
package main
import (
    "fmt"
    "sample.com/learn/math"
    "sample.com/learn/math/advanced"
)
func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math.Subtract(2, 1))
    fmt.Println(advanced.Square(2))
}
```

让我们运行这个程序。

```go
learn $ go install
learn $ learn
3
1
4
```

关于上述程序需要注意的几点。

+   我们在 main.go 中使用完整的路径导入了**advanced**包，即**import “sample.com/learn/math/advanced”**。

+   **Square**函数通过**advanced**包进行引用，即**advanced.Square(2)**。

+   如前所述，目录名称可以是其他**advanced**，只是必须相应导入。

+   此外，文件名可以是其他任何名称，而不必是**advanced.go**。

# **导入包时的别名**

在导入包时使用别名意味着为导入的包指定不同的名称。其语法为：

```go
import <new_name> <directory_path></directory_path></new_name>
```

上述语句的意思是，无论目录<directory_path>中存在什么包，都以<new_name>导入该包。别名对于命名很有用。

+   在当前上下文中为导入的包提供更相关的名称。

+   当两个不同的导入路径包含相同的包名时，则将其中一个作为不同的名称导入，以防止冲突。

让我们来看一个例子。

+   创建一个文件夹**math2\.** 创建一个文件**math2.go**，内容包含**Subtract**函数。**math2.go**中的包声明为“package math”，这意味着包名称仍为**math**。

+   **math**文件夹仍包含仅有**Add**函数的**math.go**文件。

+   请注意，包名（即**math**）在**math**文件夹和**math2\.**文件夹中是相同的。因此，文件夹**math2**和**math**都包含相同的包，即**math**。

**go.mod**

```go
module sameple.com/learn

go 1.14
```

**learn/math2/math2.go**

```go
package math
func Subtract(a, b int) int {
    return a - b
}
```

**learn/math/math.go**

```go
package math
func Add(a, b int) int {
    return a + b
}
```

**learn/main.go**

```go
package main
import (
    "fmt"
    "sample.com/learn/math"
    math2 "sample.com/learn/math2"
)
func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math2.Subtract(2, 1))
}
```

让我们运行这个程序。

```go
go install
learn $ learn
3
1
```

请注意这一行。我们将**“sample.com/learn/math2”**中存在的**math**包别名为**math2\.** 如果我们没有这样做，那么 GO 将引发编译问题，因为它无法从两个不同的文件夹导入同名的包。这是使用别名的一个优势。

```go
math2 "sample.com/learn/math2"
```

# **Init 函数**

init()函数是一个特殊函数，用于初始化包的全局变量。这些函数在包初始化时执行。包中的每个 GO 源文件都可以有自己的 init()函数。每当你在程序中导入任何包时，在该程序执行时，属于该导入包的 GO 源文件中的 init 函数（如果存在）会首先被调用。关于 init 函数需要注意的几点：

+   Init 函数是可选的。

+   Init 函数不接受任何参数。

+   Init 函数没有返回值。

+   Init 函数是隐式调用的。由于它是隐式调用的，init 函数无法从任何地方引用。

+   在同一个源文件中可以有多个 init()函数。

init 函数主要用于初始化无法通过初始化表达式初始化的全局变量。例如，它可能需要网络调用来初始化任何数据库客户端。另一个例子可能是在启动时获取密钥。init 函数还用于运行只需要执行一次的任何内容。让我们看看使用 init 函数的一个简单用例。

让我们看看一个使用 init 函数的示例。

**go.mod**

```go
module sameple.com/learn

go 1.14
```

**learn/math/add.go**

```go
package math

import "fmt"

func init(){
	fmt.Println("In add init")
}

func Add(a, b int) int {
	return a + b
}
```

**learn/math/subtract.go**

```go
package math

import "fmt"

func init(){
	fmt.Println("In subtract init")
}

func Subtract(a, b int) int {
	return a - b
}
```

**learn/main.go**

```go
package main

import (
	"fmt"

	"sample.com/learn/math"
)

func init(){
    fmt.Println("In main init")
}

func main() {
	fmt.Println(math.Add(2, 1))
	fmt.Println(math.Subtract(2, 1))
}
```

**输出**

```go
In add init
In subtract init
In main init
3
1
```

从输出中可以注意到

+   math/add.go 文件中的 init() 函数被执行，并打印 – “In add init”

+   math/subtract.go 文件中的 init() 函数被执行，并打印 – “In subtract init”

+   接下来，main.go 文件中的 init() 函数被执行，并打印 – “In main init”

尽管这两个源文件属于同一个包 math，但两个源文件中的 init 函数都被执行。

# **Go 程序的执行顺序**

以下是 Go 程序的执行顺序。

+   程序从主包开始。

+   主包源文件中所有导入的包都被初始化。后续导入的包也以递归方式执行相同的操作。

+   然后这些包中的全局变量声明被初始化。初始化依赖关系会启动以初始化这些变量。 [`golang.org/ref/spec#Order_of_evaluation`](https://golang.org/ref/spec#Order_of_evaluation)

+   此后，这些包中的 init() 函数被运行。

+   主包中的全局变量被初始化。

+   如果存在，主包中的 init 函数将被运行。

+   主包中的 main 函数将被运行。

注意，这里包的初始化只进行一次，即使被多次导入。

例如，如果主包导入包 **a**，而包 **a** 又导入包 **b**，那么顺序将如下：

+   包 **b** 中的全局变量将被初始化。包 **b** 的源文件中的 **init** 函数将被运行。

+   包 **a** 中的全局变量将被初始化。包 b 的源文件中的 **init** 函数将被运行。

+   **main** 包中的全局变量将被初始化。**main** 包源文件中的 init 函数将被运行。

+   **main** 函数将开始执行。

让我们看看一个相同的程序。

**go.mod**

```go
module sameple.com/learn

go 1.14
```

**learn/b/b1.go**

```go
package b

import (
	"fmt"
)

func init() {
	fmt.Println("Init: b1")
}

func TestB() error {
	return nil
}
```

**learn/b/b2.go**

```go
package b

import (
	"fmt"
)

func init() {
	fmt.Println("Init: b2")
}
```

**learn/a/a1.go**

```go
package a

import (
	"fmt"
	"sample/b"
)

func init() {
	fmt.Println("Init: a1")
}

func TestA() error {
	return b.TestB()
}
```

**learn/a/a2.go**

```go
package a

import (
	"fmt"
)

func init() {
	fmt.Println("Init: a2")
}
```

**learn/main.go**

```go
package main

import (
	"fmt"
	"sample/a"
)

func init() {
	fmt.Println("Init: main")
}
func main() {
	fmt.Println("Main Function Executing")
	a.TestA()
}
```

**输出**

```go
Init: b1
Init: b2
Init: a1
Init: a2
Init: main
Main Function Executing
```

请注意，在上面的示例中，包 **b** 的源文件中的 **init** 函数最先运行。然后运行包 **a** 源文件中的 **init** 函数，最后运行主包的源文件中的 **init** 函数。之后，**main** 函数开始执行。

# **导入中的空标识符**

导入包中的空标识符意味着为导入的包指定一个空导入。其语法为

```go
import _ <directory_path></directory_path>
```

这个空导入是什么，为什么会使用它。为此，你需要了解两件事。

1.  关于 **init** 函数

1.  关于由下划线（‘**_**’）表示的空标识符

**init()** 函数我们已经在上面学习过。

现在让我们来谈谈空标识符。

你已经知道 Go 不允许任何未使用的变量。任何未使用的变量可以用空标识符（‘_’）替代。

因此，现在当

+   在当前程序中，导入的包没有被使用。

+   但我们打算导入该包，以便可以调用属于该包的 GO 源文件中的 **init** 函数，并能正确初始化该包中的变量。

所以基本上，空导入是在仅为其副作用导入包时使用的。例如，MySQL 包作为空导入使用，目的是在 MySQL 包的 **init()** 函数中注册 MySQL 驱动程序作为数据库驱动程序，而不导入任何其他函数：

```go
_ "github.com/go-sql-driver/mysql"
```

# **包命名约定**

包的名称非常重要，因为访问包的类型、函数、常量或变量时都需要以包名为前缀。因此，包名应该简短且清晰。建议避免

+   包名中的下划线

+   驼峰命名法或任何混合大小写

# **结论**

在本教程中，我们学习了包并简要介绍了模块。在我们完全转向模块之前，理解包是非常重要的。在本教程的第二部分，我们将完全专注于模块。希望你喜欢这篇文章。请在评论中分享反馈/错误/改进意见。

****下一个教程** –** [包和模块 – 第二部分](https://golangbyexample.com/packages-modules-go-second/) ****上一个教程** –** [设置 GO 工作区和 Hello World 程序](https://golangbyexample.com/workspace-hello-world-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
