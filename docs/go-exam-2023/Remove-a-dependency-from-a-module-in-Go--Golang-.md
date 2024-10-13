<!--yml

类别：未分类

日期：2024-10-13 06:30:12

-->

# 从 Go 模块中移除依赖项（Golang）

> 来源：[`golangbyexample.com/remove-dependency-golang/`](https://golangbyexample.com/remove-dependency-golang/)

目录

+   概述

+   示例

# **概述**

要移除一个模块依赖项，我们需要做以下两件事

+   从模块的源文件中移除该依赖项的引用

+   运行`go mod tidy`命令。移除**go.mod**文件中不需要的所有依赖项。

# **示例**

假设我们有一个模块，导入名为“learn”的模块，包含以下`go.mod`文件和`learn.go`文件。

**go.mod**

```go
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

**learn.go**

```go
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

注意我们在**learn.go**中导入了该依赖项，并且这个依赖项也被添加到**go.mod**文件中

```go
"github.com/pborman/uuid"
```

现在让我们尝试从上述模块完全移除这个依赖项。命令**go mod tidy**将从**go.mod**文件中移除该依赖项，如果它在源文件中不需要。为了说明这一点，让我们删除之前创建的**learn.go**文件。现在运行命令

```go
go mod tidy -v
```

它将给出以下输出

```go
unused github.com/pborman/uuid
```

现在检查**go.mod**文件的内容。它将如下所示

```go
module learn

go 1.14
```

那

```go
require github.com/pborman/uuid v1.2.1
```

该行将被移除，因为在任何源文件中都不需要它。此外，**[github.com](http://github.com)/pborman/uuid**及其依赖项的所有条目也将从**go.sum**文件中移除。


