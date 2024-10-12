<!--yml
category: 未分类
date: 2024-10-13 06:29:32
-->

# Importing package within the same module in Go (Golang)

> 来源：[https://golangbyexample.com/importing-package-same-module-go/](https://golangbyexample.com/importing-package-same-module-go/)

Any package within the same module can be imported using the import path of module + directory containing that package. To illustrate lets create a module

*   Make a **learn** directory

*   Create a module with an import path as **“learn”**

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

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)