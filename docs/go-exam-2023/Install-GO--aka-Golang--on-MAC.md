<!--yml

类别：未分类

日期：2024-10-13 06:11:33

-->

# 在 MAC 上安装 GO（也称为 Golang）

> 来源：[https://golangbyexample.com/golang-mac-installation/](https://golangbyexample.com/golang-mac-installation/)

目录

**   [概述](#Overview "Overview")

+   [使用 .pkg 安装程序](#Using_pkg_installer "Using .pkg installer")

    +   [安装](#Install "Install")

    +   [测试安装：](#Test_Installation "Test Installation:")

    +   [卸载](#Uninstall "Uninstall")

+   [使用压缩包](#Using_archive "Using archive")

    +   [安装](#Install-2 "Install")

    +   [测试安装](#Test_Installation-2 "Test Installation")

    +   [卸载](#Uninstall-2 "Uninstall")

+   [使用 brew](#Using_brew "Using brew")

    +   [安装](#Install-3 "Install")

    +   [测试安装](#Test_Installation-3 "Test Installation")

    +   [卸载](#Uninstall-3 "Uninstall")*  *# **概述**

GO 可以在 Win、Mac 和 Linux 平台上安装。可以通过三种方式在 MAC 上安装 GO。

+   使用压缩包

+   使用 brew

+   使用 .pkg 安装程序

让我们看看这三种方式。

# **使用 .pkg 安装程序**

## **安装**

+   从这里下载 MAC pkg 安装程序 – [https://golang.org/dl/](https://golang.org/dl/)。双击 .pkg 文件并按照屏幕上的说明进行操作。完成后，GO 将安装在以下目录中。

```go
/usr/local/go
```

+   安装程序还会将 ‘**/usr/local/go/bin’** 添加到你的环境 PATH 变量中。这是 GO 二进制文件所在的目录。重新启动终端以使更改生效。

## **测试安装：**

+   重新启动终端并在终端中输入命令 **‘which go’**。它将输出 **/usr/local/go/bin/go.** 这是 GO 二进制文件的位置。

+   尝试运行 **‘go version’** 命令。它将输出当前的 GO 版本。

## **卸载**

要卸载，请执行以下两个步骤：

```go
rm -rf /usr/local/go      //Will require sudo permission
rm -rf /etc/paths.do/go   //Will require sudo permission. This action deletes will remove /usr/local/go/bin from PATH env
```

# **使用压缩包**

## **安装**

+   从这里下载最新版本的 GO 压缩包 – [https://golang.org/dl/](https://golang.org/dl/)。下载后在 **/usr/local** 位置解压。你可以运行以下命令进行解压。

```go
tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz
```

+   解压后，以下路径将包含 GO 二进制文件 **‘/usr/local/go/bin’**。你必须将此位置添加到你的 **.bashrc**。打开你的 **.bashrc** 并进行以下条目

```go
export PATH=$PATH:/usr/local/go/bin
```

## **测试安装**

+   重新启动终端并在终端中输入命令 **‘which go’**。它将输出 **/usr/local/go/bin/go.** 这是 GO 二进制文件的位置。

+   尝试运行 **‘go version’** 命令。它将输出当前的 GO 版本。

+   尝试运行 **‘go’** 命令。它将输出。

```go
Go is a tool for managing Go source code.

Usage:

go <command></command> [arguments]
.....
```

## **卸载**

要卸载，请执行以下两个步骤。

+   运行以下命令以删除文件。这将需要 sudo 权限。

```go
rm -rf /usr/local/go 
```

+   从 **.bashrc** 文件中删除以下条目。

```go
export PATH=$PATH:/usr/local/go/bin
```

# **使用 brew**

## **安装**

在 MAC 上安装 GO 的最简单方法是使用 brew。

```go
brew install go
```

## **测试安装**

+   重新启动终端并在终端中输入命令 **‘which go’**。它将输出 **/usr/local/go/bin/go.** 这是 GO 二进制文件的位置。

+   尝试运行 **‘go version’** 命令。它将输出当前的 GO 版本。

## **卸载**

要卸载，只需运行命令。

```go
brew uninstall go
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
