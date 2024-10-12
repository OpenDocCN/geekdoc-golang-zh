<!--yml
category: 未分类
date: 2024-10-13 06:30:08
-->

# Package vs Module in Go (Golang)

> 来源：[https://golangbyexample.com/package-vs-module-golang/](https://golangbyexample.com/package-vs-module-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Before Modules World](#Before_Modules_World "Before Modules World")
    *   [Pre Go version 1.11](#Pre_Go_version_111 "Pre Go version 1.11")
    *   [In Go version 1.11](#In_Go_version_111 "In Go version 1.11")
    *   [After Go Version 1.13](#After_Go_Version_113 "After Go Version 1.13")
*   [Creating Modules](#Creating_Modules "Creating Modules")*  *# **Overview**

As per module definition, it is a directory containing a collection of nested and related go packages **go.mod** at its root.  The **go.mod** file defines the

*   Module import path.

*   Dependency requirements of the module for a successful build. It defines both project’s dependencies requirement and also locks them to their correct version

Modules provides

*   Dependency Management

*   With modules go project doesn’t necessarily have to lie the **$GOPATH/sr**c folder. 

Also in addition to **go.mod** file go also keeps a **go.sum** file which contains the cryptographic hash of bits of all project’s dependent modules. This to make validate that your project’s dependent modules are not changed.

The behaviour of packages inside a module is same as earlier. So whatever applied for a package also applies now. There is no change in that. However a collection of packages can be called as module when there is a requirement to version them separately. Also when it is common piece of code and you want to share that code across multiple projects.

# **Before Modules World**

Let’s see version wise changes to fully understand what was the limitations earlier and what has changed since modules

*   Pre Go version 1.11 – Modules did not exist at all
*   Go Version 1.11 – Modules was introduced but not finalized
*   Go Version 1.13 – Modules was introduced

## **Pre Go version 1.11**

Before modules go only had packages. $GOPATH location would have three directories

*   src
*   pkg
*   bin

These were the problems which existed before the modules era

*   All Go project in $GOPATH/src directory

*   No native dependency management support

*   All the dependency will be downloaded in the $GOPATH/src  directory without versioning

Let’s look at each problem one by one

*   Any GO project has to be inside the $GOPATH/src directory

This was a big limitation in terms of that it restricted where you can keep your project.

*   No native dependency management support

Also, one issue before modules was that there was no way to specify a dependency in a project. Alternate solutions such as dep, glide were available but a native solution was missing.

*   All the dependency will be downloaded in the $GOPATH/src  directory without versioning

When we do a go get it will download the required package in the $GOPATH/src directory.  Run the below go get command.

```
go get github.com/pborman/uuid
```

It will download the package at location

```
$GOPATH/src/github.com/pborman/uuid
```

Notice one thing about the go get command above that no version was specified. Hence it downloads the latest version present. Also notice the downloaded package. Even it doesn’t list down any versioning info. Now this is a problem. What if there is an update in the  **[github.com](http://github.com)/pborman/uuid** package and you want to get that update. Since there is no versioning the updated package will get downloaded at the same location replacing the older one.

## **In Go version 1.11**

In Go 1.11\. modules was introduced but not finalized. So if you still using it then better to switch to the latest version

## **After Go Version 1.13**

We already discussed all the problems which existed before the pre-modules era. Now let’s see how each of these problems got resolved with the introduction of modules.

The first problem was

*   All Go project in $GOPATH/src directory

With modules this is no longer a requirement.

*   No native dependency management support

Modules introduced native dependency management within go. With modules it provides two new files that will lie

1.  go.mod
2.  go.sum

With **go.mod** and **go.sum** files we are able to install precise version of a dependency without breaking anything. We already gave a brief introduction of these files in the beginning of this tutorial. Later in the tutorial we will look at it in detail

*   All the dependency will be downloaded in the **$GOPATH/pkg/mod**  directory with versioning

So if you download a different version of the same library then both will be downloaded in a different directory inside **$GOPATH/pkg/mod** without overriding each other.  . **$GOPATH/pkg/mod**  will have two things inside it

1.  cache – This is the folder where all dependencies will get downloaded along with the zipped code
2.  Zipped code of all downloaded dependencies will get copied over from the cache directory.

Let’s now create a module. What ever we discussed will be more clear then

# **Creating Modules**

Below command can be used to create a module.

```
go mod init {module_import_path}
```

Let’s create a module

```
go mod init learn
```

This command will create a **go.mod** file in the same directory. Now what is **go.mod** file

Let’s examine the contents of this file. Do a cat **go.mod**

```
module learn

go 1.14
```

It contains the module import path and version of the go with which module was created.

Since it is an empty module it doesn’t have any direct dependency specified yet. Let’s create a file named **uuid.go** in the same directory with below contents

**uuid.go**

```
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

Notice that we have imported the dependency in the uuid.go as well

```
"github.com/pborman/uuid"
```

Let’s run the below command

```
go mod tidy
```

This command will download all the dependencies that are required in your source files and update **go.mod** file with that dependency. After running this command let’s now let’s again examine the contents of **go.mod** file. Do a cat **go.mod**

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

It lists direct dependency which was specified in the uuid file along with exact version of the dependency as well. Now let’s check the **go.sum** file as well.

Do a **cat go.sum**

```
github.com/google/uuid v1.0.0 h1:b4Gk+7WdP/d3HZH8EJsZpvV7EtDOgaZLtnaNGIu1adA=
github.com/google/uuid v1.0.0/go.mod h1:TIyPZe4MgqvfeYDBFedMoGGpEw/LqOeaOT+nhxU+yHo=
github.com/pborman/uuid v1.2.1 h1:+ZZIw58t/ozdjRaXh/3awHfmWRbzYxJoAdNJxe/3pvw=
github.com/pborman/uuid v1.2.1/go.mod h1:X/NO0urCmaxf9VXbdlT7C2Yzkj2IKimNn4k+gtPdI/k=
```

**go.sum** file lists down the checksum of direct and indirect dependency required by the module.  [github.com](http://github.com)/google/uuid is internally used by the [github.com](http://github.com)/pborman/uuid . It is an indirect dependency of the module and hence it is recorded in the **go.sum** file.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*