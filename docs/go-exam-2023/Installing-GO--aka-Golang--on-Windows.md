<!--yml

类别：未分类

日期：2024-10-13 06:11:42

-->

# 在 Windows 上安装 GO（又名 Golang）

> 来源：[`golangbyexample.com/golang-windows-installation/`](https://golangbyexample.com/golang-windows-installation/)

GO 可以在 Win、Mac 和 Linux 平台上安装。让我们来看一下 Windows 的安装设置。

**安装**

+   从这里下载.msi 安装程序 – [`golang.org/dl/`](https://golang.org/dl/)。双击.msi 文件并按照屏幕上的指示进行操作。一旦完成，GO 将安装在以下目录中。

```go
c:\Go
```

+   安装程序还会将**‘c:\Go\bin’**目录添加到你的 PATH 环境变量中。这是 GO 二进制文件所在的目录。你需要重新启动命令提示符以使更改生效。

**测试安装**

+   尝试运行**‘go version’**命令。它会输出当前的 GO 版本。

+   也尝试运行**‘go’**命令。它将输出

```go
Go is a tool for managing Go source code.

Usage:

go <command></command> [arguments]

The commands are:

bug         start a bug report
build       compile packages and dependencies
clean       remove object files and cached files
doc         show documentation for package or symbol
env         print Go environment information
fix         update packages to use new APIs
fmt         gofmt (reformat) package sources
generate    generate Go files by processing source
get         add dependencies to current module and install them
install     compile and install packages and dependencies
list        list packages or modules
mod         module maintenance
run         compile and run Go program
test        test packages
tool        run specified go tool
version     print Go version
vet         report likely mistakes in packages

Use "go help <command></command>" for more information about a command.

Additional help topics:

buildmode   build modes
c           calling between Go and C
cache       build and test caching
environment environment variables
filetype    file types
go.mod      the go.mod file
gopath      GOPATH environment variable
gopath-get  legacy GOPATH go get
goproxy     module proxy protocol
importpath  import path syntax
modules     modules, module versions, and more
module-get  module-aware go get
module-auth module authentication using go.sum
module-private module configuration for non-public modules
packages    package lists and patterns
testflag    testing flags
testfunc    testing functions

Use "go help <topic>" for more information about that topic.</topic>
```

**卸载**

要卸载，请执行以下两个步骤。

+   删除目录**– c:\Go**。

+   从 PATH 环境变量中删除条目**‘c:\Go\bin’**。请参见此链接 – [`johnatten.com/2014/12/07/adding-and-editing-path-environment-variables-in-windows/`](http://johnatten.com/2014/12/07/adding-and-editing-path-environment-variables-in-windows/)
