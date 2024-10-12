<!--yml
category: 未分类
date: 2024-10-13 06:11:42
-->

# Installing GO (aka Golang) on Windows

> 来源：[https://golangbyexample.com/golang-windows-installation/](https://golangbyexample.com/golang-windows-installation/)

GO is available to be installed on Win, Mac, and Linux platforms. Let’s see the installation set up for Windows

**Install**

*   Download the .msi  installer from here – [https://golang.org/dl/](https://golang.org/dl/). Double click the .msi file and follow the onscreen instructions. Once finished GO will be installed in the below directory.

```
c:\Go
```

*   The installer will also add **‘c:\Go\bin’** directory in your PATH environment variable. This is the directory where the GO binary resides. You have to restart command prompt for changes to take effect

**Test Installation**

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

*   Delete the directory **– c:\Go**

*   Remove the entry **‘c:\Go\bin’** from the PATH env variable. See this link – [http://johnatten.com/2014/12/07/adding-and-editing-path-environment-variables-in-windows/](http://johnatten.com/2014/12/07/adding-and-editing-path-environment-variables-in-windows/)