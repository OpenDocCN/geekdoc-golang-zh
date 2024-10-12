<!--yml
category: 未分类
date: 2024-10-13 06:29:45
-->

# Executable and non-executable module in Go (Golang)

> 来源：[https://golangbyexample.com/type-module-golang/](https://golangbyexample.com/type-module-golang/)

Module is a directory containing nested go packages. So essentially module can be treated as a package only which contains the nested package. Now package can be either executable package or utility package (non-executable). Similar to package, modules can be of two types.

*   **Executable module** – We already know that **main** is the executable package in GoLang. Hence a module containing the main package is the executable module.  The main package will contain a main function that denotes the start of a program. On installing the module having the **main** package it will create an executable in the $GOBIN directory.

*   **Non-Executable module or Utility Module**– Any package other than **main** package is a non-executable package. It is not self-executable. It just contains the utility function and other utility things that can be utilized by an executable package. Hence if the module doesn’t contain the **main** package then it will be a non-executable or utility module.  This module is meant to be used as a utility and will be imported by other modules.

Let’s see an example to understand both. We have to create a module that can be used by others (Non-Executable module or Utility Module) and a module(Executable module) that can import that module. For that let’s create two modules

*   **sample.com/math** module – This will be the Non-Executable module or Utility Module

*   **school** module – This will be the executable module. This will contain the main package and main function

**school** module will be calling code of the **sample.com/math** module

Let’s first create the **sample.com/math** module which will be used by **school** module

*   Make a **math** directory
*   Create a module with an import path as **sample.com/math**

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

Nowlet’s create the school module

*   Create a **school** directory in the same path as **math** directory side by side

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

*   Create file **school.go** which is going to use the **Add** function in **[sample.com](http://sample.com)/math** module

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

So essentially

*   **sample.com/math** is a Non-Executable module or Utility Module

*   **school** module is a executable module

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)