<!--yml

类别：未分类

日期：2024-10-13 06:31:06

-->

# 设置 GO 工作区和 Hello World 程序

> 来源：[`golangbyexample.com/workspace-hello-world-golang/`](https://golangbyexample.com/workspace-hello-world-golang/)

这是 golang 综合教程系列的第三章。请参考此链接获取系列的其他章节 – [Golang 综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程** – [包和模块 – 第一部分](https://golangbyexample.com/packages-modules-go-first)

**上一个教程** – [GO 安装](https://golangbyexample.com/golang-installation)

现在让我们查看当前的教程。以下是当前教程的目录。

目录

**   概述

+   GO 工作区

    +   GOROOT

    +   GOPATH

    +   GOBIN

+   运行 Hello World

+   结论：*  *# **概述**

这是 golang 综合教程系列的第三个教程。请参考此链接获取系列的其他章节。

在第一章中，我们看到了 Go 的介绍和 GO 的基本优点，而在第二章中，我们已经看到如何安装 GO。现在是时候为 GO 设置工作区并运行我们的第一个 Hello World 程序了。

# **GO 工作区**

在继续之前，让我们理解一些重要的 GO 环境变量：

## **GOROOT**

它是你的 GO SDK 的位置。GO 二进制发行版假设这个位置为 Linux/MAC 平台的**/usr/local/go**和 Windows 的 c:\Go。但 GO SDK 也可以安装在自定义位置。如果安装在自定义位置，则 GOROOT 应指向该目录。GOROOT 仅在将 GO 安装到自定义位置或希望维护不同版本的 GO 时设置。

例如，如果你在 Linux/MAC 平台上将 GO 安装到位置~/Documents/go，那么需要在~/$HOME/.profile 文件中添加以下条目。

```go
export GOROOT=~/Documents/go
export PATH=$PATH:$GOROOT/bin
```

## **GOPATH**

对于**GOPATH**的讨论将围绕你使用的 GO 版本是 1.13 及以上还是低于 1.13。在 1.13 版本中，GO 引入了一种新的依赖管理特性，称为 GO 模块。我们首先了解**GOPATH**的传统行为，然后再讨论在 GO 1.13 版本后与**GOPATH**有关的变化。

**在 GO 1.13 版本之前**

**GOPATH**环境变量用于解析 go 导入语句，同时还指定你的 GO 工作区的位置。它是 GO 工作区的根目录。如果依赖项不在 GOPATH 内，则无法导入。因此，早期版本要求你的所有源代码都在**GOPATH**内。所以基本上**GOPATH**是用来

+   解析导入。如果依赖项不在 GOPATH 内，则无法导入。因此，它仍然要求你的代码在 GOPATH 内。

+   包安装在目录$GOPATH/pkg/$GOOS_$GOARCH 中。

+   将编译的应用程序二进制文件存储在$GOPATH/bin 中（可以通过将$GOBIN 设置为不同路径来覆盖）。

它包含以下文件夹。

+   **src** –

    +   源文件位置。它包含.go 和其他源文件。

    +   使用‘go get’安装任何依赖包时，所有包文件都存储在此位置。

+   **pkg** –

    +   存储你在 src 目录中实际源代码的编译输出。它包含.a 文件。

    +   基本上，它包含从 src/目录编译的 GO 包。然后在链接时使用它们来创建二进制可执行文件，这些文件将放置在 bin/目录中。

    +   编译一个包一次并用于创建不同的二进制可执行文件是个好主意。

    +   每对操作系统和架构将在 pkg 中有自己的子目录（例如：pkg/GOOS_GOARCH）。

+   **bin** – Go 构建的可执行文件位置。

**使用 Go 版本 1.13 或更高版本**。

在版本**1.13**中，GO 宣布了一种新的依赖管理特性，称为 GO 模块。我们将在接下来的教程中学习这一特性。现在，你可以想象，有了新特性，GO 不再要求将所有 GO 代码放在 Go 工作空间或$GOAPTH/src 目录中。你可以在任何地方创建目录并将 Go 程序放在那里。请注意，GOPATH 的所有传统行为在 1.1.3 及更高版本中仍然有效。同时请注意，使用这个新的 GO 模块特性，GO 程序可以以两种方式运行。

+   **使用模块**：在使用模块时，**GOPATH**不用于解析导入。然而，在使用模块运行 GO 程序时，GOPATH 将被使用。

    +   将下载的源代码存储在$GOPATH/pkg/mod 目录中。

    +   将编译的应用程序二进制文件存储在$GOPATH/bin 中（可以通过将$GOBIN 设置为不同路径来覆盖）。

+   **不使用模块**：即使是 1.13 版本或更高版本，仍然可以以传统方式运行 GO 程序。在不使用模块的情况下运行 GO 程序时，$GOPATH 的行为与早期版本相同，如上所述。

**设置 GOPATH。**

如果此环境变量未设置，则在 Unix 系统上默认为**$HOME/go**，在 Windows 上默认为**%USERPROFILE%\go**。如果你的工作空间位置是~**/Desktop/go**，那么在**~/$HOME/.profile**文件中添加以下条目是个好主意。始终设置**GOPATH**是个好主意，因为即使引入模块，**GOPATH**也会被使用。

```go
export GOPATH=~/Desktop/go
```

## **GOBIN**

**GOBIN**环境变量指定了 go 在构建主包后放置编译应用程序二进制文件的目录。如果**GOPATH**环境变量未设置，则默认为**$GOPATH/bin**或**$HOME/go/bin**。此外，将**GOBIN**路径添加到**PATH**环境中也是个好主意，这样安装的二进制文件可以在不指定完整路径的情况下运行。在**~/.$HOME/.profile**文件中设置 GOBIN，并将其添加到 PATH 中。

```go
export GOBIN=$GOPATH/bin
export PATH=$PATH:$GOBIN
```

# **运行 Hello World**

在这个例子中，我们将创建我们的第一个 hello world 程序。目前**GOROOT**、**GOPATH**和**GOBIN**的值如下。

```go
echo $GOROOT will give installed directory for GO
echo $GOPATH will give ~/Desktop/go
echo $GOBIN will give ~/Desktop/go/bin
```

一个典型的程序具有**.go**文件扩展名。现在让我们创建一个“Hello World”程序。为此，首先在$GOPATH/src 外部创建一个名为 hello 的目录。由于项目是在$GOPATH/src 外部创建的，我们还需要创建一个导入路径为**[sample.com](http://sample.com)/hello**的**go.mod**文件。我们将在即将到来的教程中详细了解导入路径、go.mod 文件和模块。现在，让我们创建一个简单的模块，以便查看 hello world 程序在 go 中的样子。请使用以下命令：

```go
go mod init sample.com/hello
```

它将创建如下的 go.mod 文件。

**go.mod**

```go
module sample.com/hello

go 1.14
```

现在创建 hello.go 文件，内容如下。

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello World")
}
```

运行上述 GO 程序有三种不同的命令。首先切换到**‘hello’**目录。

1.  ‘**go install’**

它将编译程序并将二进制文件放置在**$GOBIN**文件夹中。输入以下命令，确保你在包含**hello.go**文件的**hello**目录中。

它将在$GOBIN 目录中创建一个名为 hello 的二进制文件。二进制文件的名称与模块的导入路径最后一部分相同。模块的导入路径是**[sample.com](http://sample.com)/hello**，导入路径的最后一部分简单地是**hello**。因此，在我们的例子中，二进制文件名称将是**hello**。在终端中输入 hello，将会输出如下内容：

```go
Hello World
```

‘which hello’的输出将是**‘~/Desktop/go/bin/hello’**。记住我们的**GOBIN**路径是‘**~/Desktop/go/bin**’。因此，二进制文件 hello 被复制到此目录。

1.  ‘**go build’**

它将编译程序并将二进制文件放置在当前工作目录中。输入以下命令：

```go
go build
```

在我们的例子中，它将创建一个名为**‘hello’**的二进制文件，位于**hello.go**文件旁边。要运行该二进制文件，请在终端中输入**‘./hello’**。将会输出如下内容：

```go
Hello World
```

1.  **‘go run hello.go’**

该命令将编译并执行二进制文件。输入以下命令。

```go
go run hello.go 
```

它将输出。

```go
Hello World
```

# **结论：**

本教程到此结束。希望你喜欢这篇文章。请在评论中分享反馈/错误/改进建议。

****下一个教程** –** [包和模块 – 第一部分](https://golangbyexample.com/packages-modules-go-first) ****上一个教程** –** [GO 安装](https://golangbyexample.com/golang-installation)*
