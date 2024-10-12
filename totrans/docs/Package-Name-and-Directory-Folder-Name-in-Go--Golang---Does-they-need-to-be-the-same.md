<!--yml
category: 未分类
date: 2024-10-13 06:30:35
-->

# Package Name and Directory/Folder Name in Go (Golang)- Does they need to be the same

> 来源：[https://golangbyexample.com/package-folder-name-golang/](https://golangbyexample.com/package-folder-name-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

Package name and the name of the directory containing that package don’t necessarily have to be the same. Then what does the import path of a package specify? For example in case we any package, then the import path might like as below

```
import "xyz.com/abc/sample"
```

Above statement essentially means that import package present at directory **“sample”.** It doesn’t mean import package **sample**. To validate that let’s see an example.

# **Example**

Let’s first create a directory named **learn** and then create a module with import path as **“sample.com/learn”.**

```
go mod create sample.com/learn
```

Let’s create a directory named **math** inside the **learn** directory. Create a file **math.go** inside the **math** directory with below contents

**learn/math/math.go**

```
package mathematics

func Add(a, b int) int {
    return a + b
}
```

Notice the package declaration above

```
package mathematics
```

The package name is **mathematics** but the name of the directory containing that package is **math**.

Now let’s use the **mathematics** package in the **main.go** file.

**learn/main.go**

```
package main

import (
    "fmt"
    "sample.com/learn/math"
)

func main() {
    fmt.Println(mathematics.Add(2, 1))
}
```

In **main.go** above, see how we import the package.

```
"sample.com/learn/math"
```

Above statement essentially means that import package present at directory **“math”.** It doesn’t mean import package **math.**

See how in the **main** function we are using the package.

```
fmt.Println(mathematics.Add(2, 1))
```

This is what we meant when we said  import means to import package present at that directory location.

If you run this program, output will be correct.

```
3
```

This shows that name of the package doesn’t necessarily needs to be same as the name of the directory containing the package. Another convenient way is to use package aliasing in such case like below

```
import (
    "fmt"
    mathematics "sample.com/learn/math"
)
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*