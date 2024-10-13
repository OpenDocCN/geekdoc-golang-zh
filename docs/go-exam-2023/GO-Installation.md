<!--yml

分类：未分类

日期：2024-10-13 06:31:18

-->

# GO安装

> 来源：[https://golangbyexample.com/golang-installation/](https://golangbyexample.com/golang-installation/)

这是Golang综合教程系列的第二章。请参考此链接了解系列的其他章节 – [Golang综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程** – [设置GO工作空间和Hello World程序](https://golangbyexample.com/workspace-hello-world-golang/)

**前一个教程** – [关于Golang](https://golangbyexample.com/about-golang/)

现在让我们查看当前的教程。以下是当前教程的目录。

目录

**   [概述](#Overview "Overview")

+   [在MAC上安装](#Installation_on_MAC "Installation on MAC")

    +   [使用.pkg安装程序](#Using_pkg_installer "Using .pkg installer")

    +   [使用归档](#Using_archive "Using archive")

    +   [使用brew](#Using_brew "Using brew")

+   [在Linux和Free BSD上安装](#Installation_on_Linux_and_Free_BSD "Installation on Linux and Free BSD")

    +   [下载归档](#Downloading_the_archive "Downloading the archive")

+   [在Windows上安装](#Installation_on_Windows "Installation on Windows")

    +   [使用.msi安装程序](#Using_msi_installer "Using .msi installer")

+   [结论](#Conclusion "Conclusion")*  *# **概述**

GO可以在Windows、Mac和Linux平台上安装。你可以从GO的官方下载页面下载最新GO版本的二进制文件 – [https://golang.org/dl/](https://golang.org/dl/)

GO可以在Windows、Mac和Linux平台上安装。

# **在MAC上安装**

GO可以在MAC上通过三种方式安装。

+   使用归档

+   使用brew

+   使用.pkg安装程序

让我们看看这三种方式。

## **使用.pkg安装程序**

**安装**

+   从这里下载MAC pkg安装程序 – [https://golang.org/dl/](https://golang.org/dl/)。双击.pkg文件并按照屏幕上的说明操作。完成后，GO将安装在以下目录中。

```go
/usr/local/go
```

+   安装程序还会将‘**/usr/local/go/bin’**添加到你的环境PATH变量中。这是GO二进制文件所在的目录。重新启动终端以使更改生效。

**测试安装：**

+   重新启动终端并在终端中输入命令**‘which go’**。它会输出/usr/local/go/bin/go。这是go二进制文件的位置。

+   尝试运行**‘go version’**命令。它会输出当前的GO版本。

**卸载**

卸载请执行以下两个步骤：

```go
rm -rf /usr/local/go      //Will require sudo permission
rm -rf /etc/paths.do/go   //Will require sudo permission. This action deletes will remove /usr/local/go/bin from PATH env
```

## **使用归档**

**安装**

+   从这里下载最新版本GO的归档 – [https://golang.org/dl/](https://golang.org/dl/)。下载后在**/usr/local**位置解压。你可以运行以下命令进行解压。

```go
tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz
```

+   解压后，以下路径将包含go二进制文件**‘/usr/local/go/bin’**。你必须将此位置添加到你的**.bashrc**。打开你的**.bashrc**并做以下条目。

```go
export PATH=$PATH:/usr/local/go/bin
```

**测试安装**

+   重新启动终端并在终端中输入命令 **‘which go’**。它会输出 /usr/local/go/bin/go。 这是 GO 二进制文件的位置。

+   尝试运行 **‘go version’** 命令。它会输出当前的 GO 版本。

+   也尝试运行 **‘go’** 命令。它会输出

```go
Go is a tool for managing Go source code.

Usage:

go <command></command> [arguments]
.....
```

**卸载**

要卸载，请执行以下两个步骤。

+   运行以下命令以删除文件。这将需要 sudo 权限。

```go
rm -rf /usr/local/go 
```

+   从 **.bashrc** 文件中删除以下条目。

```go
export PATH=$PATH:/usr/local/go/bin
```

## **使用 brew**

**安装**

在 MAC 上安装 GO 最简单的方法是使用 brew。

```go
brew install go
```

**测试安装**

+   重新启动终端并在终端中输入命令 **‘which go’**。它会输出 **/usr/local/go/bin/go.** 这是 GO 二进制文件的位置。

+   尝试运行 **‘go version’** 命令。它会输出当前的 GO 版本。

**卸载**

要卸载，只需运行命令。

```go
brew uninstall go
```

# **在 Linux 和 Free BSD 上安装**

## **下载归档文件**

**安装**

+   从这里下载最新版本的 GO 归档文件 – [https://golang.org/dl/](https://golang.org/dl/)。下载后在 **/usr/local** 位置解压。你也可以运行以下命令进行解压。

+   解压后，以下路径包含 GO 二进制文件 **‘/usr/local/go/bin’**。你必须将此位置添加到你的 .bashrc 中。打开你的 .bashrc，添加以下条目。如果文件尚不存在，请创建它。

**测试安装：**

+   重新启动终端并在终端中输入命令 **‘which go’**。它会输出 **/usr/local/go/bin/go.** 这是 GO 二进制文件的位置。

+   尝试运行 **‘go version’** 命令。它会输出当前的 GO 版本。

+   也尝试运行 **‘go’** 命令。

**卸载**

要卸载，请执行以下两个步骤。

+   运行以下命令以删除文件。这将需要 sudo 权限。

+   从 .bashrc 文件中删除以下条目。

```go
export PATH=$PATH:/usr/local/go/bin
```

# **在 Windows 上安装**

## **使用 .msi 安装程序**

**安装**

+   从这里下载 .msi 安装程序 – [https://golang.org/dl/](https://golang.org/dl/)。双击 .msi 文件并按照屏幕上的说明进行操作。完成后，GO 将安装在以下目录中。

```go
c:\Go

```

+   安装程序还会将 **‘c:\Go\bin’** 目录添加到你的 PATH 环境变量中。这是 GO 二进制文件所在的目录。你必须重新启动命令提示符以使更改生效。

**测试安装**

+   尝试运行 **‘go version’** 命令。它会输出当前的 GO 版本。

+   也尝试运行 **‘go’** 命令。

**卸载**

要卸载，请执行以下两个步骤。

+   删除目录 **– c:\Go**。

+   从 PATH 环境变量中删除条目 **‘c:\Go\bin’**。查看此链接 – [http://johnatten.com/2014/12/07/adding-and-editing-path-environment-variables-in-windows/](http://johnatten.com/2014/12/07/adding-and-editing-path-environment-variables-in-windows/)

# **结论**

以上就是关于 golang 安装的所有内容。希望你喜欢这篇文章。请在评论中分享反馈或错误或改进意见。

**下一个教程** – [设置Go工作区和hello world程序](https://golangbyexample.com/workspace-hello-world-golang)

**前一个教程** – [关于Golang](https://golangbyexample.com/about-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
