<!--yml

category: 未分类

date: 2024-10-13 06:29:45

-->

# Go (Golang)中的可执行和非可执行模块

> 来源：[https://golangbyexample.com/type-module-golang/](https://golangbyexample.com/type-module-golang/)

模块是包含嵌套Go包的目录。因此，模块本质上可以被视为仅包含嵌套包的包。现在，包可以是可执行包或实用包（非可执行）。类似于包，模块也可以分为两种类型。

+   **可执行模块** – 我们已经知道**main**是GoLang中的可执行包。因此，包含main包的模块就是可执行模块。main包将包含一个主函数，表示程序的开始。安装包含**main**包的模块时，它将在$GOBIN目录中创建一个可执行文件。

+   **非可执行模块或实用模块**– 除**main**包外的任何包都是非可执行包。它不是自我可执行的。它仅包含可以被可执行包使用的实用函数和其他实用内容。因此，如果模块不包含**main**包，则它将是非可执行或实用模块。此模块旨在作为实用工具使用，将被其他模块导入。

让我们看一个例子以理解两者。我们需要创建一个可供他人使用的模块（非可执行模块或实用模块）和一个可以导入该模块的模块（可执行模块）。为此，让我们创建两个模块

+   **sample.com/math**模块 – 这将是非可执行模块或实用模块

+   **school**模块 – 这将是可执行模块。它将包含main包和主函数

**school**模块将调用**sample.com/math**模块的代码

首先创建将由**school**模块使用的**sample.com/math**模块

+   创建一个**math**目录

+   创建一个导入路径为**sample.com/math**的模块

```
go mod init sample.com/math
```

+   在**math**目录中创建一个名为**math.go**的文件，内容如下

```
package math

func Add(a, b int) int {
	return a + b
}
```

现在让我们创建school模块

+   在与**math**目录并排的同一路径下创建一个**school**目录

+   创建一个模块名为**school**

```
go mod init school
```

+   现在让我们修改**go.mod**文件，以在school模块中导入math模块。为了导入未推送到VCS的本地模块，我们将使用replace目录。replace目录将用您指定的路径替换模块路径。

```
module school

go 1.14

replace sample.com/math => ../math
```

+   创建文件**school.go**，将使用**[sample.com](http://sample.com)/math**模块中的**Add**函数

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

现在执行`go run`

```
go run school.go
```

它能够调用**[sample.com](http://sample.com)/math**模块的Add函数，并正确输出为6。

所以本质上

+   **sample.com/math**是一个非可执行模块或实用模块

+   **school**模块是一个可执行模块

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/)
