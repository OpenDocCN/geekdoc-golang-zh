<!--yml
category: 未分类
date: 2024-10-13 06:30:26
-->

# Packages and Modules in Go (Golang) – Part 1

> 来源：[https://golangbyexample.com/packages-modules-go-first/](https://golangbyexample.com/packages-modules-go-first/)

This is the chapter 4 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [Packages and Modules – Part 2](https://golangbyexample.com/packages-modules-go-second/)
**Previous Tutorial** – [Set up GO Workspace and Hello World Program](https://golangbyexample.com/workspace-hello-world-golang/)

Now let’s check out the current tutorial. Below is the table of contents for the current tutorial.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Modules](#Modules "Modules")
*   [Before Modules World](#Before_Modules_World "Before Modules World")
    *   [Pre Go version 1.11](#Pre_Go_version_111 "Pre Go version 1.11")
    *   [In Go version 1.11](#In_Go_version_111 "In Go version 1.11")
    *   [After Go Version 1.13](#After_Go_Version_113 "After Go Version 1.13")
*   [Understanding Package and Also Creating a module](#Understanding_Package_and_Also_Creating_a_module "Understanding Package and Also Creating a module")
*   [Creating an application before Module](#Creating_an_application_before_Module "Creating an application before Module")
*   [Exported vs Unexported Names](#Exported_vs_Unexported_Names "Exported vs Unexported Names")
*   [Nested Packages](#Nested_Packages "Nested Packages")
*   [Aliasing in importing packages](#Aliasing_in_importing_packages "Aliasing in importing packages")
*   [Init Functions](#Init_Functions "Init Functions")
*   [Order of execution of a Go program](#Order_of_execution_of_a_Go_program "Order of execution of a Go program")
*   [Blank Identifier in import](#Blank_Identifier_in_import "Blank Identifier in import")
*   [Package Naming Convention](#Package_Naming_Convention "Package Naming Convention")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

Package is a way of code reusability in GO. As the name suggests, it is a way of grouping related code. Go modules is a way of dealing with dependencies in golang. 

Every GO source file (.go file) in a GO application file belongs to a package. That is why every **.go** file starts with.

```
package <package_name></package_name>
```

Above is called **Package Declaration**. Please note this term as this will be used throughout this tutorial.

All .go files present in the same directory will belong to the same package. A great misconception around packages is that package is the name of the directory which contains .go files. That is not correct. A directory is just a directory and the name of the package is what is present in **Package Declaration**. Then what is the importance of the directory name? It will be explained in the tutorial as we go along.

Package can be of two types.

*   **Executable package** – Only **main** is the executable package in GoLang. A .go file might belong to the main package present within a specific directory. We will see later how the directory name or the **.go** file name matters.  The main package will contain a main function that denotes the start of a program. On installing the main package it will create an executable in the **$GOBIN** directory.

*   **Utility package**– Any package other than the main package is a utility package. It is not self-executable. It just contains the utility function and other utility things that can be utilized by an executable package.

# **Modules**

Module is go support for dependency management. A module by definition is a collection of related packages with **go.mod** at its root.  The **go.mod** file defines the

*   Module import path.

*   Dependency requirements of the module for a successful build. It defines both project’s dependencies requirement and also locks them to their correct version

Consider a module as a directory containing a collection of packages. The packages can be nested as well. Modules provide

*   Dependency Management

*   With modules go project doesn’t necessarily have to lie in the **$GOPATH/sr**c folder. 

Also in addition to **go.mod** file go also keeps a **go.sum** file which contains the cryptographic hash of bits of all project’s dependent modules. This to make validate that your project’s dependent modules are not changed.

# **Before Modules World**

Modules were introduced in go in version 1.11\. Let’s see version wise changes to fully understand what was the limitations earlier and what has changed since modules

*   Pre Go version 1.11 – Modules did not exist at all

*   Go Version 1.11 – Modules was introduced but not finalized

*   Go Version 1.13 – Modules was introduced

## **Pre Go version 1.11**

Before modules go only had packages. $GOPATH location would have three directories

*   src
*   pkg
*   bin

These were the problems which existed before the modules era

*   All Go project in **$GOPATH/src** directory

*   No native dependency management support

*   All the dependency will be downloaded in the **$GOPATH/src**  directory without versioning

Let’s look at each problem one by one:

*   Any GO project has to be inside the **$GOPATH/src** directory

This was a big limitation in terms of that it restricted where you can keep your project.

*   No native dependency management support

Also, one issue before modules was that there was no way to specify a dependency in a project. Alternate solutions such as dep, glide were available but a native solution was missing.

*   All the dependency will be downloaded in the $GOPATH/src  directory without versioning

When we do a go get it will download the required package in the $GOPATH/src directory.  So before go version 1.11, below go get command

```
go get github.com/pborman/uuid
```

It will download the package at the location

```
$GOPATH/src/github.com/pborman/uuid
```

Notice one thing about the go get command above that no version was specified. Hence it downloads the latest version present. Also, notice the downloaded package. Even it doesn’t list down any versioning info. Now, this is a problem. What if there is an update in the  **[github.com](http://github.com)/pborman/uuid** package and you want to get that update. Since there is no versioning the updated package will get downloaded at the same location replacing the older one.

## **In Go version 1.11**

In Go 1.11\. modules were introduced but not finalized. So if you still using it then better to switch to the latest version

## **After Go Version 1.13**

We already discussed all the problems which existed before the pre-modules era. Now let’s see how each of these problems got resolved with the introduction of modules.

The first problem was

*   All Go project in **$GOPATH/src** directory

With modules, this is no longer a requirement.

*   No native dependency management support

Modules introduced native dependency management within go. With modules, it provides two new files which are

1.  go.mod
2.  go.sum

With **go.mod** and **go.sum** files we are able to install a precise version of a dependency without breaking anything. We already gave a brief introduction to these files at the beginning of this tutorial. Later in the tutorial, we will look at it in detail

*   All the dependency will be downloaded in the **$GOPATH/pkg/mod**  directory with versioning

So if you download a different version of the same library then both will be downloaded in a different directory inside **$GOPATH/pkg/mod** without overriding each other.  **$GOPATH/pkg/mod**  will have two things inside it

1.  **cache** – This is the folder where all dependencies will get downloaded along with the zipped code
2.  Zipped code of all downloaded dependencies will get copied over from the cache directory.

Also, there is a new env introduced named **GO111MODULE**

When **GO111MODULE=off**, then go get will behave in the old way where it will download the dependency in the $GOPATH/src folder.

When **GO111MODULE=on**, then go get will behave in a new way and all the modules will get downloaded in the **$GOPATH/pkg/mod/cache** folder with versioning.

When **GO111MODULE=auto**, then

*   When running go get outside the $GOPATH/src folder, then will behave as if it is GO111MODULE=on

*   When running go get inside the $GOPATH/src folder, then will behave as if it is GO111MODULE=off

Let’s now create a module. Whatever we discussed will be more clear then

# **Understanding Package and Also Creating a module**

The below command can be used to create a module.

```
go mod init {module_import_path}
```

Let’s see **go.mod** and **go.sum** file again that we had discussed earlier

**go.mod**

It is the module dependency file. It will have three things

*   Import path of the module at the top

*   The version of go with which the module is created

*   Direct dependencies of the module.

**go.sum**

This file lists down the checksum of direct and indirect dependency required along with the version. It is to be mentioned that **go.mo**d file is enough for a successful build. The checksum present in **go.sum** file is used to validate the checksum of each direct and indirect dependency.

Now the question is what is **import_path**.  **import_path** is the prefix path that will be used by any other module to import your module. We will learn more about the import path in the second part of the tutorial.

Go to any directory outside $GOPATH/src folder. Let’s say the directory name is **learn**. 

```
mkdir learn
cd learn
```

Let’s say the module import path is **sample.com/learn**

```
go mod init sample.com/learn
```

This command will create a **go.mod** file in the same directory.

Let’s examine the contents of this file. Do a cat **go.mod**

```
module sameple.com/learn

go 1.14
```

When the module is first created using the init command, go.mod file will have two things only

*   Import path of the module at the top

```
module sameple.com/learn
```

*   Version of go with which the module was created

```
go 1.14
```

Since it is an empty module it doesn’t have any direct dependency specified yet. Let’s create a file named **main.go** in the same directory with below contents

**main.go**

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

Notice the package declaration in above file

```
package main
```

This means that above source file belongs to the **main** package. Also Notice that we have imported the dependency in the **main.go** as well

```
"github.com/pborman/uuid"
```

Since **main.go** file belongs to main package, hence we can create an executable. The executable is always built with the last name in the import path of the module which contains the main package. while running the go install command. In our case the import path of the module is sample.com/learn and the last name in the import path is learn. Hence executable will be created with name learn when running go install.

Any of three command can be used to create the executable

*   **‘go install learn’**
*   **‘go install’**
*   **‘go install .’**

The name of the directory would not matter here. The name of the executable will always be the same as the name of the module.  All the above commands will create an executable name **learn** in the **$GOBIN** directory. If the **$GOBIN** directory is in your path then you can directory run the executable

```
learn
```

Also go install command will download all the dependencies that are required in your source files and update **go.mod** file with that dependency.

After running this command let’s now let’s again examine the contents of **go.mod** file

Do a cat **go.mod**

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

It lists direct dependency which was specified in the **main.go** file along with exact version of the dependency as well. Now let’s check the **go.sum** file as well.

Do a cat **go.sum**

```
github.com/google/uuid v1.0.0 h1:b4Gk+7WdP/d3HZH8EJsZpvV7EtDOgaZLtnaNGIu1adA=
github.com/google/uuid v1.0.0/go.mod h1:TIyPZe4MgqvfeYDBFedMoGGpEw/LqOeaOT+nhxU+yHo=
github.com/pborman/uuid v1.2.1 h1:+ZZIw58t/ozdjRaXh/3awHfmWRbzYxJoAdNJxe/3pvw=
github.com/pborman/uuid v1.2.1/go.mod h1:X/NO0urCmaxf9VXbdlT7C2Yzkj2IKimNn4k+gtPdI/k=
```

**go.sum** file lists down the checksum of direct and indirect dependency required by the module.  [github.com](http://github.com)/google/uuid is internally used by the [github.com](http://github.com)/pborman/uuid . It is an indirect dependency of the module and hence it is recorded in the **go.sum** file. Both the direct dependency as well as the indirect dependency will be downloaded in the $GOAPTH/pkg/mod/cache folder with versioning.

We can also run the executable directly.

```
learn
```

Note: **$GOBIN** directly needs to be in your path for above command to run.

**Output**

```
e594dc4d9a754bcb83b56e89b18b4b46
```

The above way we added a dependency in the source file and when we run go install it downloaded that dependency and added it in the go.mod file.  Also the executable got created in the **$GOBIN** directory.

Now couple of points to notice

*   Name of the directory is **learn**

Name of the directory which contains the **go.mod** file doesn’t matter. You can try changing name to anything. It will always create the executable with the same name as last part in module import path.

*   Import path of the module is **sample.com/learn**

The executable is always created with the name of the last part in the module import path which is **learn** here. How does import path matter we will learn in the second part of the tutorial. For now, just understand that the module import path is used to import that module into another module. Also if the module import path is just one name then executable will be created with that name only. For example, it is possible that the module import path would only have been just **learn.** In this case, too executable would have been created with name learn only. So for the below module import paths executable name would be **learn**

```
sample.com/manage/learn
sample.com/learn
learn
```

*   Name of file is **main.go**

The name of the file won’t matter here when running ‘go install’. You can try changing the file from main.go to test.go. After doing that run the above commands again. It will create an executable with the module name which is **learn** and nothing changes.

*   Name of package in learn module is **main**

The name of the package does matter. If go install sees a main package in the directory, it will create an executable in the **$GOBIN** directory. The name of the executable will be the same as the last name in the module import path.

What we learnt so far:

*   Only **main** package is executable.

*   The name of the directory containing the module doesn’t play any role in the name of the executable.

*   On running **‘go install**‘ it creates the binary with the name as the last part of the module import that contains the .go file belonging to the main package.

*   The name of the file doesn’t matter when running go install. You can have any name

Let’s also see the same example before modules to understand the benefits offered with the introduction of the module.

# **Creating an application before Module**

Before module all your code should lie in the **$GOPATH/src** folder. Let’s create a directory **learn** inside **$GOPATH/src** directory.

**$GOPATH/src/learn/main.go**

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

Run

```
export GO111MODULE=off
go install
```

Before modules the executable is always build with the name of the directory which contains the **main** package while running the go install command. In our case the name of directory is **learn**

All the above commands will create an executable name **learn** in the $GOBIN directory. If the $GOBIN directory is in your path then you can directory run the executable

```
learn
```

Let’s see the limitations before modules

*   All code needs to reside in the **$GOPATH/src/** folder

*   When you do a go install, it will download both direct and indirect dependency in the **$GOPATH/src** folder without any versioning information.  You can check your  **$GOPATH/src** folder. It would have downloaded the **“github.com/pborman/uuid”** dependency without any versioning

*   There is no way to define the dependency version within code. A simple go get will download the latest version of the dependency and save it at location **$GOPATH/src** overriding the previous version if already present.

Modules solve all this problem as we have seen in the first example. Now we have understood the basics of packages and modules and what advantages do modules provide. Let’s now focus only on understanding the package more in detail here in this tutorial. We will focus more on modules in the second part of this tutorial- modules. Here is the link for second part – [https://golangbyexample.com/packages-modules-go-second](https://golangbyexample.com/packages-modules-go-second)/

Let’s now add another file belonging to the main package.

**learn/subtract.go**

```
package main
func subtract(a, b int) int {
    return a - b
}
```

**learn/main.go**

```
package main
import "fmt"
func main() {
    fmt.Println(add(2, 1))
    fmt.Println(subtract(2, 1))
}
func add(a, b int) int {
    return a + b
}
```

Try running the go install command again. It will create an executable with name **learn** in the **$GOBIN** directory. Try running this executable ,It will give below output. Also notice function **main** in **main.go** is able to call the function **subtract** in **subtract.go**. It is possible because both **main** and **subtract** belong to same package main.

```
learn $ learn
3
-1
```

So basically

*   Within the same package, all variables, functions, constants are accessible between different .go files belonging to the package.

All **.go** files present in the same directory will belong to the same package. This is true for all directory containing packages. It doesn’t matter whether that directory contains the **go.mod** file or not.   Let’s validate that. Change the package declaration to

```
package subtract
```

**go.mod**

```
module sameple.com/learn

go 1.14
```

**learn/subtract.go**

```
package subtract

func subtract(a, b int) int {
    return a-b
}
```

**learn/main.go**

```
package main

import "fmt"

func main() {
    fmt.Println(add(2, 1))
    fmt.Println(subtract(2, 1))
}

func add(a, b int) int {
    return a + b
}
```

Notice the **Package Declaration.** It is

```
package subtract
```

Let’s try running the **go install** command. It will give error

```
can't load package: package learn: found packages subtract (subtract.go) and main (main.go) in
```

The above error means that directory **learn** contains two files **subtract.go** and **main.go** belonging to two different packages named **subtract** and **main** respectively.  GO source files belonging to different packages with in the same directory is not allowed hence this error. Change the **Package Declaration** in subtract.go to belong to **main** package and the error will go away

As you might have noticed we are using two functions, **add** and **subtract.** These two methods are arithmetic functions and it will be good if we can make as part of some common code which can be used by other parts. This is where package is useful . Idea is to create a new package **math** which will include these two functions.  Through this way package provides a way of code reusability. Let’s introduce a new package **math**

**go.mod**

```
module sameple.com/learn

go 1.14
```

**learn/math/math.go**

```
package math

func Add(a, b int) int {
    return a + b
}
func Subtract(a, b int) int {
    return a - b
}
```

**learn/main.go**

```
package main

import (
    "fmt"
    "sample.com/learn/math"
)

func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math.Subtract(2, 1))
}
```

If you run this program, output will be same:

```
3
1
```

Couple of points to notice about above program

*   **Importing of a package**

main.go file imports the package using **“sample.com/learn/math”** and is able to call Add and Subtract using math.Add(..) and math.Subtract(..)See how we have imported the math package in the main.go file.

```
"sample.com/learn/math"
```

Here the import path is import path of module which is **learn** +  directory containing the package which is **math.** Hence **“sample.com/learn/math”** . Packages in nested directory can also be imported in the same way.

*   **Creation of a package**

We created new directory math and created new file math.go in it. The math.go has Package Declaration as “package math”. It also has file **math.go** which contains two functions **Add** and **Subtract**.

Let’s talk about the import statement first. There are two ways in which we can import a package

*   A short hand for import

```
import (
    "fmt"
    "sample.com/learn/math"
)
```

and another form

```
import "fmt"
import "sample.com/learn/math"
```

You can import any number of packages in a program. GO tries to resolve in below way

*   Check at directory $GOROOT/src – “fmt” package lies at location **$GOROOT/src/fmt**

*   With modules, it resolves the path starting from the root of the module i.e, the directory which contains the **go.mod** file.

```
import "sample.com/learn/math"
```

Above statement essentially means that import package present at location **“sample.com/learn/math”.** It doesn’t mean import package **math**. To validate that let’s change the directory name to **math2\. Package Declaration** is still same which is **“package math”**

**go.mod**

```
module sample.com/learn

go 1.14
```

**learn/math2/math.go –** Notice that directory name is **math2** here

```
package math

func Add(a, b int) int {
    return a + b
}

func Subtract(a, b int) int {
    return a - b
}
```

**learn/main.go**

```
package main

import (
    "fmt"
    "sample.com/learn/math2"
)

func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math.Subtract(2, 1))
}
```

If you run this program, output will still be same:

In **main.go** the import folder changes to **“sample.com/learn/math2”** but it  **main** function in **main.go** still refers to **Add** and **Subtract** as **math.Add()** and **math.Subtract()** . So basically GO imports **math** package from **“math2”** directory. This is what we meant when we said  import means to import package present at that directory location.

So we now know  the use of a package directory name

*   Use of the directory is for import statements in GO program. In import, we provide the directory path and not the package name. GO then fetches all the files having the same package name.

What about filename. In above case filename is **math.go.** Filename doesn’t matter. You can try changing filename from **math.go** to something else. Output will still the same.

Let’s move ahead. You might have noticed that both **Add** and **Subtract** are capitalized. Why is that. What will happen if they are not capitalized. If Add and Subtract were not capitalized, then **main.go** won’t be able to refer them. This brings our next topic for discussion.

# **Exported vs Unexported Names**

Go doesn’t have any **public**,  **private** or **protected** keyword. The only mechanism to control the visibility is using the capitalized and non-capitalized formats

*   **Capitalized Identifiers** are exported. The capital letter indicates that this is an exported identifier. It will be visible in other packages 

*   **Non-capitalized identifiers** are not exported. The lowercase indicates that the identifier is not exported and will only be accessed from within the same package.

There are five kinds of identifier which can be exported or non-exported

1.  Structure
2.  Structure’s Method
3.  Structure’s Field
4.  Function
5.  Variable

Let’s add a function named **multiply** which will be in lowercase. We will add it to math package and see if it is accessible from **main.go**.

**go.mod**

```
module sameple.com/learn

go 1.14
```

**learn/math/math.go**

```
package math
func Add(a, b int) int {
    return a + b
}
func Subtract(a, b int) int {
    return a - b
}
func multiply(a, b int) int {
    return a * b
}
```

**learn/main.go**

```
package main
import (
    "fmt"
    "sample.com/learn/math"
)
func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math.Subtract(2, 1))
    fmt.Println(math.multiply(2, 1))
}
```

Let’s run this program. It will give error

```
learn $ go install
learn $ learn
./main.go:12:14: cannot refer to unexported name math.multiply
./main.go:12:14: undefined: math.multiply
```

The error is because **main.go** cannot refer to unexported name **math.multiply**. Make multiply as uppercase and it will work. It should give below output after that.

```
3
1
2
```

Also as mentioned during the start of the tutorial, **multiply** function will be available within the **math** package. To illustrate this let’s create a function **Mul** in arithmetic.go which will call **multiply** function internally

**go.mod**

```
module sameple.com/learn

go 1.14
```

**learn/math/math.go**

```
package math
func Add(a, b int) int {
    return a + b
}
func Subtract(a, b int) int {
    return a - b
}
func Mul(a, b int) int {
    return multiply(a, b)
}
func multiply(a, b int) int {
    return a * b
}
```

**learn/main.go**

```
package main
import (
    "fmt"
    "sample.com/learn/math"
)
func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math.Subtract(2, 1))
    fmt.Println(math.Mul(2, 1))
}
```

Let’s run this program

```
learn $ go install
learn $ learn
3
1
2
```

The above program works because within the same package you can refer to unexported or non-capitalized names as well.

For now **math.go** contains both **Add** and **Subtract** function. Can we distribute these two functions to different files with these function still belonging to same package **math.** Yes we can do that. Let’s create **add.go** and **subtract.go** file inside the **math** directory. add.go will contain the **Add** function and subtract.go will contain the **Subtract** function. Notice that both **add.go** and **subtract.go** have the same package declaration which is **package math**

**go.mod**

```
module sameple.com/learn

go 1.14
```

**learn/math/add.go**

```
package math
func Add(a, b int) int {
    return a + b
}
```

**learn/math/subtract.go**

```
package math
func Subtract(a, b int) int {
    return a - b
}
```

**learn/main.go**

```
package main
import (
    "fmt"
    "sample.com/learn/math"
)
func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math.Subtract(2, 1))
}
```

Let’s run this program

```
learn $ go install
learn $ learn
3
1
```

It works because the import statement in **main.go** fetches all identifiers from all files which belong to package **math** from import. path **“sample.com/learn/math”**

# **Nested Packages**

In GO it is possible to create nested packages. Let’s create a new directory named **advanced** inside **math** directory.  “**.** This directory will contain **square**.go file which will package declaration as **“package advanced”**

**go.mod**

```
module sameple.com/learn

go 1.14
```

**learn/math/math.go**

```
package math
func Add(a, b int) int {
    return a + b
}
func Subtract(a, b int) int {
    return a - b
}
```

**learn/math/advanced/advanced.go**

```
package advanced
func Square(a int) int {
    return a * a
}
```

**learn/main.go**

```
package main
import (
    "fmt"
    "sample.com/learn/math"
    "sample.com/learn/math/advanced"
)
func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math.Subtract(2, 1))
    fmt.Println(advanced.Square(2))
}
```

Let’s run this program

```
learn $ go install
learn $ learn
3
1
4
```

Points to note about above program

*   We imported the **advanced** package in main.go with full qualified path i.e,  **import “sample.com/learn/math/advanced”**

*   **Square** function is referred using **advanced** package i.e, **advanced.Square(2)**

*   As mentioned earlier directory name can be other **advanced** just that it has to be imported accordingly

*   Also filename can be anything other than **advanced.go**

# **Aliasing in importing packages**

Aliasing in importing packages means give a different name to the imported package. Syntax for it is

```
import <new_name> <directory_path></directory_path></new_name>
```

Above statement means that what ever package is present at directory <directory_path> import that package with name <new_name>. Aliasing is useful for giving

*   Giving more relevant name to the imported package in the current context.

*   When two different import path contains same package name then import one of them as a different name to prevent conflict.

Let’s see an example.

*   Create a folder **math2\.** Create a file **math2.go** with the below contents having the **Subtract** function. Package Declaration in **math2.go** is “package math” which means the package name is still **math**

*   **math** folder still contains **math.go** file with the only **Add** function

*   Notice that package name (i.e **math**) is same in folder **math** as well in folder **math2\.** So both folder **math2** and **math** contains the same package which is **math**

**go.mod**

```
module sameple.com/learn

go 1.14
```

**learn/math2/math2.go**

```
package math
func Subtract(a, b int) int {
    return a - b
}
```

**learn/math/math.go**

```
package math
func Add(a, b int) int {
    return a + b
}
```

**learn/main.go**

```
package main
import (
    "fmt"
    "sample.com/learn/math"
    math2 "sample.com/learn/math2"
)
func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math2.Subtract(2, 1))
}
```

Let’s run this program

```
go install
learn $ learn
3
1
```

Notice the line. We alias **math** package present **“sample.com/learn/math2”** to **math2\.** If we haven’t done that then GO will raise a compilation issue as it cannot import package with same name from two different folders. This one of the advantage of using alias.

```
math2 "sample.com/learn/math2"
```

# **Init Functions**

init() function is a special function that is used to initialize global variables of a package. These functions are executed when the package is initialized. Each of the GO source files in a package can have its own init() function. Whenever you import any package in the program, then on the execution of that program, init functions(if present)  in the GO source files belonging to that imported package are called first. Some points to note about init function

*   Init function is optional

*   Init function does not take any argument

*   Init function does not have any return value.

*   Init function is called implicitly. Since it is called implicitly, init function cannot reference it from anywhere.

*   There can be multiple init() functions within the same source file.

init function is majorly used for the initialization of global variables that cannot be initialized using an initialization expression. For example, it requires a network call to intialize any DB client. Another example could be fetching secret keys on startup. Init function is also used for running anything that only needs to be executed once. Let’s see a simple use case of using an init function. 

Let’s see an example of using an init function.

**go.mod**

```
module sameple.com/learn

go 1.14
```

**learn/math/add.go**

```
package math

import "fmt"

func init(){
	fmt.Println("In add init")
}

func Add(a, b int) int {
	return a + b
}
```

**learn/math/subtract.go**

```
package math

import "fmt"

func init(){
	fmt.Println("In subtract init")
}

func Subtract(a, b int) int {
	return a - b
}
```

**learn/main.go**

```
package main

import (
	"fmt"

	"sample.com/learn/math"
)

func init(){
    fmt.Println("In main init")
}

func main() {
	fmt.Println(math.Add(2, 1))
	fmt.Println(math.Subtract(2, 1))
}
```

**Output**

```
In add init
In subtract init
In main init
3
1
```

Notice from the output that

*   init() function in the math/add.go file is executed and it prints – “In add init”

*   init() function in the math/subtract.go file is executed next and it prints – “In subtract init”

*   init() function in the main.go file is executed next and it prints – “In main init”

Although both the source files belong to the same package math but init function in both the source files gets executed.

# **Order of execution of a Go program**

Below the order of execution of a go program.

*   The program starts with the main package.

*   All imported packages in the source files of the main package are initialized. The same thing happens recursively for further imported packages.

*   Then global variables declaration in these packages is initialized. The initialization dependency kicks in for the initialization of these variables. [https://golang.org/ref/spec#Order_of_evaluation](https://golang.org/ref/spec#Order_of_evaluation)

*   After this, init() function is run in these packages

*   Global variables in the main package are initialized

*   init function in the main package is run if present

*   main function in main package is run.

Note here that package initialization is only done once even if it is imported several times. 

For example, if the main package imports package **a** and in turn package **a** imports package **b**, then below will be the order

*   Global variables in package **b** will be initialized. **init** function in source files of package **b** will be run

*   Global variables in the package **a** will be initialized. **init** function in source files of package b will be run

*   Global variables in the **main** package will be initialized. init function in source files of the **main** package will be run

*   The **main** function will start executing.

Let’s see a program for the same.

**go.mod**

```
module sameple.com/learn

go 1.14
```

**learn/b/b1.go**

```
package b

import (
	"fmt"
)

func init() {
	fmt.Println("Init: b1")
}

func TestB() error {
	return nil
}
```

**learn/b/b2.go**

```
package b

import (
	"fmt"
)

func init() {
	fmt.Println("Init: b2")
}
```

**learn/a/a1.go**

```
package a

import (
	"fmt"
	"sample/b"
)

func init() {
	fmt.Println("Init: a1")
}

func TestA() error {
	return b.TestB()
}
```

**learn/a/a2.go**

```
package a

import (
	"fmt"
)

func init() {
	fmt.Println("Init: a2")
}
```

**learn/main.go**

```
package main

import (
	"fmt"
	"sample/a"
)

func init() {
	fmt.Println("Init: main")
}
func main() {
	fmt.Println("Main Function Executing")
	a.TestA()
}
```

**Output**

```
Init: b1
Init: b2
Init: a1
Init: a2
Init: main
Main Function Executing
```

Notice in the above example that the **init** function in source files of package **b** is run first. Then **init** function in source files of the package **a** is run and then **init** function in the source file of the main package is run. After that **main** function is run

# **Blank Identifier in import**

Blank identifier in importing packages means specifying a blank import for the imported package. The syntax for it is

```
import _ <directory_path></directory_path>
```

What is this blank import and why it is used. For this, you have to understand two things

1.  About **init** function
2.  About blank identifier represented by an underscore (‘**_**‘)

**init()** function we already studied above.

Now let’s come to blank identifier.

You are already aware that go doesn’t allow any unused variable. Any unused variable can be replaced by a blank identifier (‘_’).
So now a blank import of a package is used when

*   The imported package is not being used in the current program

*   But we intend to import that package so that the **init** function in the GO source files belonging to that package can be called and initialization of variables in that package can be done properly

So basically a blank import is used when a package is solely imported for its side effects. As an example MySQL package is used as a blank import for its side-effect of registering the MySQL driver as a database driver in the **init()** function of MySQL package, without importing any other functions:

```
_ "github.com/go-sql-driver/mysql"
```

# **Package Naming Convention**

A good name is very important for the package as any access to the package’s types, functions, constants, or variables is prefixed by the package name. So the package name should be short and clear. It is recommended to avoid

*   Underscore in the package name
*   Camel casing or any kind of mixed caps

# **Conclusion**

In this tutorial, we learned about packages and gave a small introduction to modules. It is important to understand packages first before we can jump fully to modules. In the second part of this tutorial, we will focus entirely on modules. Hope you have liked this article. Please share feedback/mistakes/improvements in the comments

****Next Tutorial** –** [Packages and Modules – Part 2](https://golangbyexample.com/packages-modules-go-second/) ****Previous Tutorial** –** [Set up GO Workspace and Hello World Program](https://golangbyexample.com/workspace-hello-world-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*