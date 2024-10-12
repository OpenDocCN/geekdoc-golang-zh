<!--yml
category: 未分类
date: 2024-10-13 06:29:14
-->

# Modules in Go (Golang)

> 来源：[https://golangbyexample.com/modules-golang/](https://golangbyexample.com/modules-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Before Modules World](#Before_Modules_World "Before Modules World")
    *   [Pre Go version 1.11](#Pre_Go_version_111 "Pre Go version 1.11")
    *   [In Go version 1.11](#In_Go_version_111 "In Go version 1.11")
    *   [After Go Version 1.13](#After_Go_Version_113 "After Go Version 1.13")
*   [Creating Modules](#Creating_Modules "Creating Modules")
*   [Add a dependency to your project](#Add_a_dependency_to_your_project "Add a dependency to your project")
    *   [Directly adding it to the go.mod file](#Directly_adding_it_to_the_gomod_file "Directly adding it to the go.mod file")
    *   [Do a go get](#Do_a_go_get "Do a go get")
    *   [Add the dependency to your source code and do a go mod tidy](#Add_the_dependency_to_your_source_code_and_do_a_go_mod_tidy "Add the dependency to your source code and do a go mod tidy")
*   [Adding a vendor directory](#Adding_a_vendor_directory "Adding a vendor directory")
*   [Module Import Path](#Module_Import_Path "Module Import Path")
    *   [The module is a utility module and you plan to publish your module](#The_module_is_a_utility_module_and_you_plan_to_publish_your_module "The module is a utility module and you plan to publish your module")
    *   [The module is a utility module and you don’t plan to publish your module](#The_module_is_a_utility_module_and_you_dont_plan_to_publish_your_module "The module is a utility module and you don’t plan to publish your module")
    *   [The module is a executable module](#The_module_is_a_executable_module "The module is a executable module")
*   [Importing package within same module](#Importing_package_within_same_module "Importing package within same module")
*   [Importing package from different module locally](#Importing_package_from_different_module_locally "Importing package from different module locally")
*   [Selecting the version of library](#Selecting_the_version_of_library "Selecting the version of library")
    *   [Differ in minor or patch version](#Differ_in_minor_or_patch_version "Differ in minor or patch version")
    *   [Differ in major version](#Differ_in_major_version "Differ in major version")
*   [go mod command](#go_mod_command "go mod command")
*   [Direct vs Indirect Dependencies in go.mod file](#Direct_vs_Indirect_Dependencies_in_gomod_file "Direct vs Indirect Dependencies in go.mod file")
    *   [Example of Indirect Dependencies in go.mod file](#Example_of_Indirect_Dependencies_in_gomod_file "Example of Indirect Dependencies in go.mod file")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

Module is go support for dependency management. A module by definition is a collection of related packages with **go.mod** at its root.  The **go.mod** file defines the

*   Module import path.

*   Dependencies requirements of the module for a successful build. It defines both project’s dependencies requirement and also locks them to their correct version.

Consider module as a directory containing a collection of packages. The packages can be nested as well. Modules provides

*   Dependency Management

*   With modules go project doesn’t necessarily have to lie the **$GOPATH/sr**c folder. 

Also in addition to **go.mod** file go also keeps a **go.sum** file which contains the cryptographic hash of bits of all project’s dependent modules. This to make validate that your project’s dependent modules are not changed.

# **Before Modules World**

Let’s see version wise changes to fully understand what was the limitations earlier and what has changed since modules

*   Pre Go version 1.11 – Modules did not existed at all

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

Modules introduced native dependency management within go. With modules, it provides two new files that will lie

1.  go.mod
2.  go.sum

With go.mod and go.sum files we are able to install precise version of a dependency without breaking anything.We already gave a brief introduction of these files in the beginning of this tutorial. Later in the tutorial we will look at it in detail

*   All the dependency will be downloaded in the **$GOPATH/pkg/mod**  directory with versioning

So if you download different version of the same library then both will be downloaded in different directory inside **$GOPATH/pkg/mod** without overriding each other.  . **$GOPATH/pkg/mod**  will have two things inside it

*   cache – This is the folder where all dependencies will get downloaded along with the zipped code

*   Zipped code of all downloaded dependencies will get copied over from the cache directory.

Also there is a new env introduced named **GO111MODULE**

When GO111MODULE=off, then go get will behave in the old way where it will download the dependency in the $GOPATH/src folder

When GO111MODULE=on, then go get will behave in a new way and all the modules will get downloaded in the $GOPATH/pkg/mod/cache folder with versioning.
When GO111MODULE=auto, then

*   When running go get outside the $GOPATH/src folder, then will behave as if it is GO111MODULE=on

*   When running go get inside the $GOPATH/src folder, then will behave as if it is GO111MODULE=off

Let’s now create a module. What ever we discussed will be more clear then

# **Creating Modules**

Below command can be used to create a module.

```
go mod init {module_import_path}
```

Let’s see **go.mod** and **go.sum** file again that we had discussed earlier

**go.mod**

It is the module dependency file. It will have three things

*   Name of the module at the top

*   Version of go with which the module is created

*   Direct dependencies of the module.

**go.sum**

This file lists down the checksum of direct and indirect dependency required along with the version.It is to be mentioned that go.mod file is enough for a successful build. The checksum present in go.sum file is used to validate the checksum of each of direct and indirect dependency.

Now question is what is **import_path**.  **import_path** is the prefix path that will be used by any other module to import your module.

Go to any directory outside $GOPATH/src folder. Let’s say the directory name is **learn**. 

```
mkdir learn
cd learn
```

Let’s say module name is also **learn**

```
go mod init learn
```

This command will create a **go.mod** file in the same directory. Now what is go.mod file

Let’s examine the contents of this file

Do a **cat go.mod**

```
module learn

go 1.14
```

When the module is first created using the init command, go.mod file will have two things only

*   Name of the module at the top

```
module learn
```

Version of go with which the module was created

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

This command will download all the dependencies that are required in your source files and update **go.mod** file with that dependency. After running this command let’s now let’s again examine the contents of **go.mod** file

Do a cat **go.mod**

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

It lists direct dependency which was specified in the uuid file along with exact version of the dependency as well. Now let’s check the **go.sum** file as well

Do a **cat go.sum**

```
github.com/google/uuid v1.0.0 h1:b4Gk+7WdP/d3HZH8EJsZpvV7EtDOgaZLtnaNGIu1adA=
github.com/google/uuid v1.0.0/go.mod h1:TIyPZe4MgqvfeYDBFedMoGGpEw/LqOeaOT+nhxU+yHo=
github.com/pborman/uuid v1.2.1 h1:+ZZIw58t/ozdjRaXh/3awHfmWRbzYxJoAdNJxe/3pvw=
github.com/pborman/uuid v1.2.1/go.mod h1:X/NO0urCmaxf9VXbdlT7C2Yzkj2IKimNn4k+gtPdI/k=
```

**go.sum** file lists down the checksum of direct and indirect dependency required by the module.  [github.com](http://github.com)/google/uuid is internally used by the [github.com](http://github.com)/pborman/uuid . It is an indirect dependency of the module and hence it is recorded in the **go.sum** file.

We can also run this file now and it will give the correct output

```
go run uuid.go
```

**Output**

```
e594dc4d9a754bcb83b56e89b18b4b46
```

The above way we added a dependency in the source file and used go mod tidy command to download that dependency and add it in the go.mod file.

**Types of Modules**

We learn that module is a directory containing nested go packages. So essentially module can be treated as a package only which contains nested package. We have seen in the package tutorial can package can be either an executable package or utility package (non-executable). Similar to package, modules can be of two types.

*   **Executable module** – We already know that **main** is the executable package in GoLang. Hence a module containing the main package is the executable module.  The main package will contain a main function that denotes the start of a program. On installing the module having the **main** package it will create an executable in the **$GOBIN** directory.

*   **Non-Executable module or Utility Module**– Any package other than **main** package is a non-executable package. It is not self-executable. It just contains the utility function and other utility things that can be utilized by an executable package. Hence if the module doesn’t contain the **main** package then it will be a non-executable or utility module.  This module is meant to be used as a utility and will be imported by other modules.

To create an executable for a module  (Only for the module with the main package)

*   Do a go build and it will create the executable in the current directory

*   Do a go install and it will create the executable in the $GOBIN directory

# **Add a dependency to your project**

Let’s explore some ways of adding dependency to your project

*   Directly adding it to the **go.mod** file

*   Do a **go get**

*   Add the dependency to your source code and do a **go mod tidy**

Before looking at each of the ways, again let’s create a module first

```
go mod init learn
```

## **Directly adding it to the go.mod file**

We can  add direct dependency to the go.mod file too. Let’s do that

Add below dependency to the go.mod file

```
require github.com/pborman/uuid v1.2.1
```

With this dependency go.mod file will look like below

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

## **Do a go get**

Simply doing a go get will also the add the dependency in the go.mod file. Remove the uuid dependency we added above from go.mod file and clean up go.sum file. Now run below command

```
export GO111MODULE=on
go get github.com/pborman/uuid
```

Now check the contents of go.mod file

Do a **cat go.mod**

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1 //indirect
```

The dependency will be marked as **//indirect** as it is not being used in any of the source files. Once you do a go build after using this in the source files, the //indirect will be removed automatically by go. Also it will update the **go.sum** file with the checksum and version of all direct and indirect dependencies.

## **Add the dependency to your source code and do a go mod tidy**

This method we already saw in the above example.  Basically, go mod tidy command makes sure that your go.mod files reflects the dependencies that you have actually used in your project. When we run go mod tidy command then it will do two things

*   Add any dependency which is imported in the source files

*   Remove any dependency which is mentioned in the **go.mod** file but not imported in any of the source files.

# **Adding a vendor directory**

If you want to vendor your dependencies,  then below command can be used to achieve the same

```
go mod vendor
```

It will create a vendor directory inside your project directory. You can also check in the vendor directory to your VCS (Version Control System). This becomes useful in sense that none of the dependency needs to be downloaded at run time as it is already present in the vendor folder checked into VCS

# **Module Import Path**

We have already seen that module import path is the prefix path that is used to import all packages within that module

There can be three cases which decide what import path name can be used with modules.

*   The module is a utility module and you plan to publish your module

*   The module is a utility module and you don’t plan to publish your module

*   The module is a executable module

## **The module is a utility module and you plan to publish your module**

If you plan to publish your module then the module name should match the URL of the repo which host that module. Go tries to download dependencies from the VCS using the same import path of the module.

## **The module is a utility module and you don’t plan to publish your module**

This is the case when you only mean to use the utility module locally only. In this case the import path can be anything.

## **The module is a executable module**

In this case also module import path can be anything. The module import path can be a non-url even if you plan to commit your module into VCS as it will not be used by any other module

However it is a good practice to use meaningful import path while creating module

# **Importing package within same module**

Any package within the same module can be imported using the import path of module + directory containing that package. To illustrate lets create a module

*   Make a **learn** directory
*   Create a module with import path as “learn”

```
go mod init learn
```

*   Now create main.go (Having main package and main function)
*   And math/math.go – math package

**main.go**

```
package main

import (
	"fmt"
	"learn/math"
)

func main() {
	fmt.Println(math.Add(1, 2))
}
```

**math/math.go**

```
package math

func Add(a, b int) int {
    return a + b
}
```

See how we have imported the math package in the main.go file

```
"learn/math"
```

Here the import path is import path of module which is **learn** +  directory containing the package which is **math.** Hence “learn/math” . Packages in nested directory can also be imported in the same way. The way it works is that since the prefix is the module import path, hence go will know that you are trying to import from the same module. So it will directly refer it instead of downloading it.

# **Importing package from different module locally**

There are cases when we want to import a module which is present locally. Let’s understand how we can import such module. But first, we have to create a module that can be used by others and then import it into the other module. For that let’s create two modules

*   **sample.com/math** module

*   **school** module

**school** module will be calling code of the **sample.com/math** module

Let’s first create the **sample.com/math** module which will be used by **school** module

*   Make a **math** directory

*   Create a module with import path as **sample.com/math**

```
go mod init sample.com/math
```

*   Create a file **math.go** with below contents  in the **math** directory

```
package math

func Add(a, b int) int {
	return a + b
}
```

Now let’s create the school module

*   Nowcreate a **school** directory in the same path as **math** directory side by side

*   Create a module name **school**

```
go mod init school
```

*   Now let’s modify the **go.mod** file to import the math module in the school module. To import a local module that is not pushed to VCS, we are going to use replace directory. The replace directory will replace the module path with the path you specify.

```
module school

go 1.14

replace sample.com/math => ../math
```

*   Create file school.go which is going to use the Add function in **[sample.com](http://sample.com)/math** module

```
package main

import (
	"fmt"
	"sample.com/math"
)

func main() {
	fmt.Println(math.Add(2, 4))
}
```

Now do a go run

```
go run school.go
```

It is able to call the Add function of the **[sample.com](http://sample.com)/math** module and correctly gives the output as 6.

Also it will update the go.mod with version information of the **[sample.com](http://sample.com)/math** module

```
module school

go 1.14

replace sample.com/math => ../math

require sample.com/math v0.0.0-00010101000000-000000000000
```

# **Selecting the version of library**

To understand how does GO’s approach while selecting the version of the library of which two versions are specified in the **go.mod** file, we have to first understand Semantic VersioningSemantic Versioning is comprised of three parts separated by dots. Below is the format for versioning.

```
v{major_version}.{minor_version}.{patch_version}
```

where

*   **v** – it is just an indicator that it is a version

*   major_version – It represents the incompatible API changes in the library. So when there are changes in the library that is not backward compatible, in that case, major_version is incremented

*   minor_version – It represents the change in functionality of the library in a backward-compatible manner. So when there are some functionality changes in the library but those changes are backward compatible then, in that case, the minor version is incremented

*   patch_version – It represents the bug fixes in the library in a backward-compatible manner. So when there are bug fixes to the existing functionality of the library, then, in that case, patch_version is incremented.

Now there can be two cases

*   Two versions of the same library are used which only differ in the minor and patch version. Their major version is the same.

*   Two version of the same library is used which differ in the major.

Let’s see what approach does go follows in the above two cases

## **Differ in minor or patch version**

Go follows the minimum version policy approach while selecting the version of the library of which two versions are specified in the **go.mod** file which differ only in their minor or patch version.

For example in case you are using the two versions of same library which are

```
1.2.0
```

and

```
1.3.0
```

then go will choose 1.3.0 as it is the latest version.

## **Differ in major version**

Go treats the major version as a different module itself. Now, what does that means? This essentially means that the import path will have a major version as its suffix. Let’s take the example of any go library with VCS as **github.com/sample**. Let’s latest semantic version is

```
v8.2.3
```

Then the go.mod file will like below

```
module github.com/sample/v8

go 1.13

..
```

It has major version in its import path. So any library which is using this sample library have to import it like

```
import "github.com/sample/v8"
```

If in future **v9** version is released than it has to be imported in the application like

```
import "github.com/sample/v9"
```

Also go-redis library will change its go.mod file to reflect the v9 major version

```
module github.com/samples/v9
```

What it essentially allows is to use different major version of the same library to be used within same go application.  We can also give meaningful names when different major version of the same library is imported in the same application. For eg

```
import redis_v8 "github.com/sample/v8"
import redis_v9 "github.com/sample/v9"
```

This is also known as **Semantic Import Versioning**. Also note that

*   For the first version, it is ok to not specify the version in the go.mod file.

*   Also, be careful when importing a different major versions of the same library. Look out for the new functionality that might be available with new versions.

Also for the same reason when you update a specific module using

```
go get -u
```

then it will only upgrade to the latest minor version or patch version whichever applicable. For example let’s say the current version used by an application is

```
v1.1.3
```

Also let’s say we have below versions available

```
v1.2.0
v2.1.0
```

Then when we run

```
go get
```

then it will update to

```
v1.2.0
```

The reason is because go get will only update the minor or patch version but never the major version as go treats major version of a module as a different module entirely.

To upgrade the major version, specify that  upgraded dependency explicitly  in the **go.mod** file or do a go get of that version.

Also couple of points to note about upgrading module

*   To upgrade a dependency to its latest patch version only, use below command

```
go get -u=patch <dependency_name></dependency_name>
```

*   To upgrade a dependency to a specific version, use below command

```
go get dependency@version
```

*   To upgrade a dependency to a specific commit, use below command

```
go get <dependency_name>@commit_number</dependency_name>
```

*   To upgrade all dependency to their latest minor and patch version, use below command

```
go get ./...
```

# **go mod command**

Below are some of the options for the go mod command.

*   **download –** It will download the required dependencies to the  $GOPATH/pkg/mod/cache folder.  Also, it will update the go.sum file with the checksum and version of all direct and indirect dependencies

*   **edit** – This is for editing the go.mod file. It provides a set of editing flags. Run the below command to see the set of all editing flags available

```
go help mod edit
```

For eg below are some editing flags available

1.  **-fmt** flag will format the go.mod file. It will not make any other change
2.  **-module** flag can be used to set the module’s import path

*   **graph** – This can be used to print the module requirement dependency graph

*   **init** – We already have seen the usage of this command above. It is used to init a new module

*   **tidy** – This command will download all the dependencies that are required in your source files

*   **vendor** – If you want to vendor your dependencies,  then the below command can be used to achieve the same. It will create a vendor directory inside your project directory. You can also check in the vendor directory to your VCS (Version Control System)

*   **verify –** This command checks for the modification of currently downloaded dependencies. If any of the downloaded dependency has been verified that it will exit with a non-zero code

*   **why –** this command analyzes the graph of packages from the main module. It prints the shortest path from the main module to the given package. For instance the school module which we created in section “Importing package from different module locally” if we print why command as below

```
go mod why sample.com/math
```

then below will be the output

```
# sample.com/math
school
sample.com/math
```

The output illustrates that the [sample.com](http://sample.com)/math package is at one distance in the graph from main module which is school here.

# **Direct vs Indirect Dependencies in go.mod file**

A direct dependency is the dependency which the module directly imports . An indirect dependency is the dependency which are imported by module’s direct dependencies. Also, any dependency that is mentioned in the **go.mod** file but not imported in any of the source files of the module is also treated as an indirect dependency.

**go.mod** file only records the direct dependency. However, it may record an indirect dependency in the below cases

*   Any indirect dependency which is not listed in the go.mod file of your direct dependency or if direct dependency doesn’t have a go.mod file, then that direct dependency will be added to the go.mod file with //direct as the suffix

*   Any dependency which is not imported in any of the source files of the module (Example of this we have already seen earlier in the tutorial)

**go.sum** will record the checksum of direct and indirect dependencies.

## **Example of Indirect Dependencies in go.mod file**

Let’s understand it with an example. For that let’s first create a module

```
git mod init learn
```

Let’s add colly lib version v1.2.0 as a dependency in the go.mod file. colly version v1.2.0 doesn’t have a go.mod file.

```
module learn

go 1.14

require github.com/gocolly/colly v1.2.0
```

Now create a file learn.go

```
package main

import (
	"github.com/gocolly/colly"
)

func main() {
	_ = colly.NewCollector()
}
```

Now do a go build. Since colly version v1.2.0 doesn’t have a go.mod file , all dependencies required by colly will be added to the go.mod file with //indirect as suffix

Do a go build. Now check the go.mod file. You will see below contents of the file

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

All other dependencies are suffixed by **//indirect**. Also check that all direct and indirect dependencies will be recorded in the go.sum file.

# **Conclusion**

This is all about modules in golang. Hope you have liked this article. Please share feedback in comments

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*