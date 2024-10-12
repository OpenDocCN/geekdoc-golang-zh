<!--yml
category: 未分类
date: 2024-10-13 06:29:09
-->

# Direct vs Indirect Dependencies in go.mod file in Go

> 来源：[https://golangbyexample.com/direct-indirect-dependency-module-go/](https://golangbyexample.com/direct-indirect-dependency-module-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example of direct and indirect dependency](#Example_of_direct_and_indirect_dependency "Example of direct and indirect dependency")
*   [Example of indirect dependency in go.mod file](#Example_of_indirect_dependency_in_gomod_file "Example of indirect dependency in go.mod file")*  *# **Overview**

Module is Go support for dependency management. A module by definition is a collection of related packages with **go.mod** at its root.  The **go.mod** file defines the

*   Module import path.

*   Dependencies requirements of the module for a successful build. It defines both module’s dependencies requirement and also lock them to their correct version.

A dependency of a module can be of two kinds

*   **Direct** -A direct dependency is a dependency which the module directly imports.
*   **Indirect** – It is the dependency that is imported by the module’s direct dependencies. Also, any dependency that is mentioned in the go.mod file but not imported in any of the source files of the module is also treated as an indirect dependency.

**go.mod** file only records the direct dependency. However, it may record an indirect dependency in the below cases

*   Any indirect dependency which is not listed in the go.mod file of your direct dependency or if direct dependency doesn’t have a **go.mod** file, then that dependency will be added to the go.mod file with //indirect as the suffix
*   Any dependency which is not imported in any of the source files of the module.

**go.sum** will record the checksum of both direct and indirect dependencies

# **Example of direct and indirect dependency**

Let’s see an example of direct dependency.  For that let’s first create a module

```
git mod init learn
```

Now create a file **learn.go**

```
package main

import (
	"github.com/pborman/uuid"
)

func main() {
	_ = uuid.NewRandom()
}
```

Notice that we have specified the dependency in the **learn.go** as

```
"github.com/pborman/uuid"
```

So **[github.com](http://github.com)/pborman/uuid** is a direct dependency of the learn module as it is directly imported in the module. Now let’s run the below command

```
go mod tidy
```

This command will download all the dependencies that are required in your source files. After running this command let’s now let’s again examine the contents of **go.mod** file

Do a cat **go.mod**

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

It lists direct dependency which was specified in the **learn.go** file along with exact version of the dependency as well. Now let’s check the go.sum file as well

Do a cat **go.sum**

```
github.com/google/uuid v1.0.0 h1:b4Gk+7WdP/d3HZH8EJsZpvV7EtDOgaZLtnaNGIu1adA=
github.com/google/uuid v1.0.0/go.mod h1:TIyPZe4MgqvfeYDBFedMoGGpEw/LqOeaOT+nhxU+yHo=
github.com/pborman/uuid v1.2.1 h1:+ZZIw58t/ozdjRaXh/3awHfmWRbzYxJoAdNJxe/3pvw=
github.com/pborman/uuid v1.2.1/go.mod h1:X/NO0urCmaxf9VXbdlT7C2Yzkj2IKimNn4k+gtPdI/k=
```

**go.sum** file lists down the checksum of direct and indirect dependency required by the module.  [github.com](http://github.com)/google/uuid is internally used by the [github.com](http://github.com)/pborman/uuid and it is an indirect dependency of the module and hence it is recorded in the **go.sum** file.

The above way we added a dependency in the source file and used **go mod tidy** command to download that dependency and add it in the **go.mod** file.

# **Example of indirect dependency in go.mod file**

Let’s understand it with an example. For that let’s first create a module

```
git mod init learn
```

Now create a file **learn.go**

```
package main

import (
	"github.com/gocolly/colly"
)

func main() {
	_ = colly.NewCollector()
```

Notice that we have specified the dependency in the **learn.go** as

```
github.com/gocolly/colly
```

So github.com/gocolly/colly is a direct dependency of the learn module as it is directly imported in the module.Let’s add colly version v1.2.0 as a dependency in the go.mod file

```
module learn

go 1.14

require	github.com/gocolly/colly v1.2.0
```

Now let’s run the below command

```
go mod tidy
```

After running this command let’s now let’s again examine the contents of **go.mod** file. Since colly version v1.2.0 doesn’t have a go.mod file , all dependencies required by colly will be added to the **go.mod** file with **//indirect** as suffix

Do a **cat go.mod**

```
module learn

go 1.14

require (
	github.com/PuerkitoBio/goquery v1.6.0 // indirect
	github.com/antchfx/htmlquery v1.2.3 // indirect
	github.com/antchfx/xmlquery v1.3.3 // indirect
	github.com/gobwas/glob v0.2.3 // indirect
	github.com/gocolly/colly v1.2.0
	github.com/kennygrant/sanitize v1.2.4 // indirect
	github.com/saintfish/chardet v0.0.0-20120816061221-3af4cd4741ca // indirect
	github.com/temoto/robotstxt v1.1.1 // indirect
	golang.org/x/net v0.0.0-20201027133719-8eef5233e2a1 // indirect
	google.golang.org/appengine v1.6.7 // indirect
)
```

All other dependencies are suffixed by **//indirect**. Also checksum of all direct and indirect dependencies will be recorded in the go.sum file.

We also mentioned that any dependency which is not imported in any of the source file will be marked as //indirect. To illustrate that delete the **learn.go** created above. Also clean up **go.mod** file to remove all require lines. Now run below command

```
go get github.com/pborman/uuid
```

Now check the contents of **go.mod** file

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1 // indirect
```

Notice that the dependency is recorded as //indirect as it is not imported in any of the source files.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*