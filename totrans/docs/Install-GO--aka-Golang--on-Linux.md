<!--yml
category: 未分类
date: 2024-10-13 06:11:37
-->

# Install GO (aka Golang) on Linux

> 来源：[https://golangbyexample.com/golang-linux-installation/](https://golangbyexample.com/golang-linux-installation/)

GO is available to be installed on Win, Mac, and Linux platforms. Let’s see the installation set up for Linux

**Install**

*   Download the archive of latest version of GO from here  – [https://golang.org/dl/](https://golang.org/dl/). After downloading untar it at the location **/usr/local.** You can also run the below command to untar

```
tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz
```

*   After the untar then the below path contains the go binary **‘/usr/local/go/bin’** . You have to add this location to your .bashrc. Open your .bashrc make the below entry. If the file doesn’t already exist then create it.

```
export PATH=$PATH:/usr/local/go/bin
```

**Test Installation:**

*   Relaunch the terminal and type the  command ‘which go’ in the terminal.It will output **/usr/local/go/bin/go.** This is the location of GO binary.

*   Try running the **‘go version’** command. It will output the current GO Version

*   Try running **‘go’** command as well. It will output

```
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

**Uninstall**

To uninstall do below two steps

*   Run below command to remove the files. It will require sudo permission.

```
rm -rf /usr/local/go
```

*   Remove the below entry from the .bashrc file

```
export PATH=$PATH:/usr/local/go/bin
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [install](https://golangbyexample.com/tag/install/)