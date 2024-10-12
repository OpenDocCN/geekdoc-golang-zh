<!--yml
category: 未分类
date: 2024-10-13 06:29:50
-->

# //indirect for a dependency in go.mod file in Go (Golang)

> 来源：[https://golangbyexample.com/indirect-dependency-golang/](https://golangbyexample.com/indirect-dependency-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

As the name suggests **//indirect** against a dependency in the **go.mod** file indicates that the dependency is indirect. **go.mod** file only records the direct dependency. However, it may record an indirect dependency in the below cases

*   Any indirect dependency which is not listed in the **go.mod** file of your direct dependency or if direct dependency doesn’t have a **go.mod** file, then that dependency will be added to the go.mod file with **//indirect** as the suffix

*   Any dependency which is not imported in any of the source files of the module. That dependency will be recorded as **//indirect** if the dependency has been added via go get.

# **Example**

Let’s understand both points above with example. For that let’s first create a module

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

So **github.com/gocolly/colly** is a direct dependency of the **learn** module as it is directly imported in the module. Let’s add colly version v1.2.0 as a dependency in the go.mod file

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

require github.com/pborman/uuid v1.2.1 //indirect
```

Notice that the dependency is recorded as **//indirect** as it is not imported in any of the source files. Now create uuid.go file as below which imports the **[github.com](http://github.com)/pborman/uuid** as dependency

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

Now do a go build. After go build again check the contents of go.mod file. The **//indirect** will now be removed as the dependency is required in the source file of the module which is uuid.go

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*