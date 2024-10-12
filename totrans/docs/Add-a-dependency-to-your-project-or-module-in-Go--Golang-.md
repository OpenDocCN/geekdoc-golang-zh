<!--yml
category: 未分类
date: 2024-10-13 06:29:41
-->

# Add a dependency to your project or module in Go (Golang)

> 来源：[https://golangbyexample.com/add-dependency-module-golang/](https://golangbyexample.com/add-dependency-module-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Directly adding it to the go.mod file](#Directly_adding_it_to_the_gomod_file "Directly adding it to the go.mod file")
*   [Do a go get](#Do_a_go_get "Do a go get")
*   [Add the dependency to your source code and do a go mod tidy](#Add_the_dependency_to_your_source_code_and_do_a_go_mod_tidy "Add the dependency to your source code and do a go mod tidy")*  *# **Overview**

Module is go support for dependency management. A module by definition is a collection of related packages with **go.mod** at its root.  The **go.mod** file defines the

*   Module import path

*   Dependency requirements of the module for a successful build. It defines both module’s dependencies requirement and also locks them to their correct version.

A dependency of a module can be of two kinds

*   Direct -A direct dependency is a dependency which the module directly imports.

*   Indirect – It is the dependency that is imported by the module’s direct dependencies. Also, any dependency that is mentioned in the **go.mod** file but not imported in any of the source files of the module is also treated as an indirect dependency.

Let’s explore some ways of adding dependency to your project

*   Directly adding it to the **go.mod** file
*   Do a **go get**
*   Add the dependency to your source code and do a **go mod tidy**

Before looking at each of the ways, again let’s create a module first

```
go mod init learn
```

This command will create a **go.mod** file in the same directory.  Since it is an empty module it doesn’t have any direct dependency specified yet. Now let’s explore different ways of adding a dependency

# **Directly adding it to the go.mod file**

We can  add direct dependency to the go.mod file too. Let’s do that. Add below dependency to the **go.mod** file

```
require github.com/pborman/uuid v1.2.1
```

With this dependency **go.mod** file will look like below

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

Now we need to download the newly added dependency as well. Fo that we can use the below command

```
go mod download
```

This command will download the **github.com/pborman/uuid** module as well all its dependencies. Also it will update the **go.sum** file with the checksum and version of all direct and indirect dependencies. go build as well as go install also will download the dependencies and also build the binary. go run will also download and and run the binary as well. go mod download command is used when you want to pre download the dependencies without build or running it.

# **Do a go get**

Simply doing a go get will also the add the dependency in the go.mod file. Remove the uuid dependency we added above from go.mod file and clean up go.sum file. Now run below command

```
export GO111MODULE=on
go get github.com/pborman/uuid
```

Now check the contents of go.mod file. Do a **cat go.mod**

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1 //indirect
```

The dependency will be marked as **//indirect** as it is not being used in any of the source files. Once you do a go build after using this in the source files, the **//indirec**t will be removed automatically by go. Also it will update the **go.sum** file with the checksum and version of all direct and indirect dependencies.

# **Add the dependency to your source code and do a go mod tidy**

This method we already saw in the above example.  Basically, go mod tidy command makes sure that your go.mod files reflect the dependencies that you have actually used in your project. When we run go mod tidy command then it will do two things

*   Add any dependency which is imported in the source files

*   Remove any dependency which is mentioned in the **go.mod** file but not imported in any of the source files.

Let’s see an example. Remove the dependency we added earlier in the go.mod file. Your go.mod file should like below

```
module learn

go 1.14
```

Also let’s create a file named **uuid.go** in the same directory with below contents

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

This command will download all the dependencies that are required in your source files and update **go.mod** file with that dependency. After running this command let’s now let’s again examine the contents of **go.mod** file

Do a cat **go.mod**

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

It lists direct dependency which was specified in the uuid file along with exact version of the dependency as well. Now let’s check the **go.sum** file as well. Do a cat **go.sum**

```
github.com/google/uuid v1.0.0 h1:b4Gk+7WdP/d3HZH8EJsZpvV7EtDOgaZLtnaNGIu1adA=
github.com/google/uuid v1.0.0/go.mod h1:TIyPZe4MgqvfeYDBFedMoGGpEw/LqOeaOT+nhxU+yHo=
github.com/pborman/uuid v1.2.1 h1:+ZZIw58t/ozdjRaXh/3awHfmWRbzYxJoAdNJxe/3pvw=
github.com/pborman/uuid v1.2.1/go.mod h1:X/NO0urCmaxf9VXbdlT7C2Yzkj2IKimNn4k+gtPdI/k=
```

Now let’s see one more example where **go mod tidy** will remove the dependency from **go.mod** file if it is not required in the source files. To illustrate this let’s remove the **uuid.go** file that we created above. Now run the command

```
go mod tidy -v
```

It will give below output

```
unused github.com/pborman/uuid
```

Now examine the contents of **go.mod** file.  It will be as below

```
module learn

go 1.14
```

The

```
require github.com/pborman/uuid v1.2.1
```

line will be removed as it is not required in any of the source files. Also all entries of **[github.com](http://github.com)/pborman/uuid** and its depedencies will be removed from the **go.sum** file as well.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*