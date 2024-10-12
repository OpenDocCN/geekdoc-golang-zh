<!--yml
category: 未分类
date: 2024-10-13 06:30:03
-->

# Understanding go.sum and go.mod file in Go (Golang)

> 来源：[https://golangbyexample.com/go-mod-sum-module/](https://golangbyexample.com/go-mod-sum-module/)

**Note:** If you are interested in learning Golang, then for that we have a golang comprehensive tutorial series. Do check it out – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/). Now let’s see the current tutorial. Below is the table of contents.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")
*   [Example of indirect dependency in go.mod file](#Example_of_indirect_dependency_in_gomod_file "Example of indirect dependency in go.mod file")*  *# **Overview**

Module is go support for dependency management. A module by definition is a collection of related packages with **go.mod** at its root.  The **go.mod** file defines the

*   Module import path.

*   The version of go with which the module is created

*   Dependency requirements of the module for a successful build. It defines both project’s dependencies requirement and also locks them to their correct version.

**go.sum**

This file lists down the checksum of direct and indirect dependency required along with the version. It is to be mentioned that the **go.mod** file is enough for a successful build. They why **go.sum** file is needed?. The checksum present in **go.sum** file is used to validate the checksum of each of direct and indirect dependency to confirm that none of them has been modified.

We mentioned above that go.mod file lists down the dependency requirement of the module. Now a dependency of a module can be of two kind

*   **Direct** -A direct dependency is a dependency which the module directly imports.

*   **Indirect** – It is the dependency that is imported by the module’s direct dependencies. Also, any dependency that is mentioned in the **go.mod** file but not imported in any of the source files of the module is also treated as an indirect dependency.

**go.mod** file only records the direct dependency. However, it may record an indirect dependency in the below case

*   Any indirect dependency which is not listed in the **go.mod** file of your direct dependency or if direct dependency doesn’t have a go.mod file, then that dependency will be added to the **go.mod** file with **//indirect** as the suffix. We will see an example of this later in the article to know this better.

Also please note that both **go.mod** as well as **go.sum** file should be checked into the Version Control System (VCS) such as git

# **Example**

Let’s see an example to understand what ever we talked above about **go.mod** and **go.sum** file. For that let’s first create a module

```
go mod init learn
```

This command will create a **go.mod** file in the same directory. Let’s examine the contents of this file. Do a cat **go.mod**

```
module learn

go 1.14
```

When the module is first created using the init command, the **go.mod** file will have two things only

*   Import path of the module at the top

```
module learn
```

*   Version of go with which the module was created

```
go 1.14
```

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

This command will download all the dependencies that are required in your source files and update **go.mod** file with that dependency. After running this command let’s now let’s again examine the contents of **go.mod** file . Do a cat **go.mod**

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

It lists direct dependency which was specified in the uuid file along with exact version of the dependency as well. Now let’s check the **go.sum** file as well

Do a cat **go.sum**

```
github.com/google/uuid v1.0.0 h1:b4Gk+7WdP/d3HZH8EJsZpvV7EtDOgaZLtnaNGIu1adA=
github.com/google/uuid v1.0.0/go.mod h1:TIyPZe4MgqvfeYDBFedMoGGpEw/LqOeaOT+nhxU+yHo=
github.com/pborman/uuid v1.2.1 h1:+ZZIw58t/ozdjRaXh/3awHfmWRbzYxJoAdNJxe/3pvw=
github.com/pborman/uuid v1.2.1/go.mod h1:X/NO0urCmaxf9VXbdlT7C2Yzkj2IKimNn4k+gtPdI/k=
```

**go.sum** file lists down the checksum of direct and indirect dependency required by the module.  **[github.com](http://github.com)/google/uuid** is internally used by the **[github.com](http://github.com)/pborman/uuid** . It is an indirect dependency of the module and hence it is recorded in the **go.sum** file.

We can also run this file now and it will give the correct output

```
go run uuid.go
```

**Output**

```
e594dc4d9a754bcb83b56e89b18b4b46
```

# **Example of indirect dependency in go.mod file**

We mentioned above that **go.mod** file might contain an indirect dependency in the below case.

*   Any indirect dependency which is not listed in the **go.mod** file of your direct dependency or if direct dependency doesn’t have a go.mod file , then that dependency will be added to the **go.mod** file with **//indirect** as the suffix. We will see an example of this later in the article to know this better.

Let’s understand it with an example. For that let’s first create a module again

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
}
```

Notice that we have specified the dependency in the **learn.go** as

```
github.com/gocolly/colly
```

So **github.com/gocolly/colly** is a direct dependency of the learn module as it is directly imported in the module. Now let’s run the below command

```
go mod tidy
```

After running this command let’s now let’s again examine the contents of **go.mod** file. Since colly version v1.2.0 doesn’t have a go.mod file , all dependencies required by colly will be added to the **go.mod** file with //indirect as suffix

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

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*