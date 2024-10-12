<!--yml
category: 未分类
date: 2024-10-13 06:29:27
-->

# Importing package from different module locally in Go (Golang)

> 来源：[https://golangbyexample.com/import-local-module-golang/](https://golangbyexample.com/import-local-module-golang/)

There are cases when we want to import a module which is present locally. Let’s understand how we can import such module. But first, we have to create a module that can be used by others and then import it into the other module. For that let’s create two modules

*   **sample.com/math** module
*   **school** module

**school** module will be calling code of the **sample.com/math** module

Let’s first create the **sample.com/math** module which will be used by **school** module

*   Make a **math** directory

*   Create a module with an import path as **sample.com/math**

```
go mod init sample.com/math
```

*   Create a file **math.go** with the below contents  in the **math** directory

```
package math

func Add(a, b int) int {
	return a + b
}
```

Nowlet’s create the school module

*   Create a **school** directory in the same path as the **math** directory side by side
*   Create a module name **school**

```
go mod init school
```

*   Now let’s modify the **go.mod** file to import the math module in the school module. To import a local module that is not pushed to VCS, we are going to use the replace directory. The replace directory will replace the module path with the path you specify.

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

Also, it will update the **go.mod** with version information of the **[sample.com](http://sample.com)/math** module

```
module school

go 1.14

replace sample.com/math => ../math

require sample.com/math v0.0.0-00010101000000-000000000000
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)