<!--yml

类别：未分类

日期：2024-10-13 06:11:37

-->

# 在 Linux 上安装 GO（也称为 Golang）

> 来源：[`golangbyexample.com/golang-linux-installation/`](https://golangbyexample.com/golang-linux-installation/)

GO 可在 Windows、Mac 和 Linux 平台上安装。让我们看看 Linux 的安装设置。

**安装**

+   从这里下载最新版本的 GO 的压缩包 – [`golang.org/dl/`](https://golang.org/dl/)。下载后在**/usr/local.**位置解压。你也可以运行以下命令来解压。

```go
tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz
```

+   解压后，下面的路径包含 GO 二进制文件**‘/usr/local/go/bin’**。你必须将此位置添加到你的.bashrc 中。打开你的.bashrc 并进行以下条目。如果文件尚不存在，请创建它。

```go
export PATH=$PATH:/usr/local/go/bin
```

**测试安装：**

+   重新启动终端，并在终端中输入命令`which go`。它将输出**/usr/local/go/bin/go.** 这是 GO 二进制文件的位置。

+   尝试运行**‘go version’**命令。它将输出当前的 GO 版本。

+   尝试运行**‘go’**命令。它将输出。

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

+   运行以下命令以删除文件。这需要 sudo 权限。

```go
rm -rf /usr/local/go
```

+   从.bashrc 文件中删除以下条目。

```go
export PATH=$PATH:/usr/local/go/bin
```


