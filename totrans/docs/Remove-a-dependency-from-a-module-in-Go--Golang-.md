<!--yml
category: 未分类
date: 2024-10-13 06:30:12
-->

# Remove a dependency from a module in Go (Golang)

> 来源：[https://golangbyexample.com/remove-dependency-golang/](https://golangbyexample.com/remove-dependency-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

To remove a module dependency we need to do below two things

*   Remove reference of that dependency from the source files of the module

*   Run go mod tidy command. Remove all dependencies from the **go.mod** file which are not required in the source files.

# **Example**

Let’s say we have a module with import that as”learn” having below go.mod file and learn.go file.

**go.mod**

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

**learn.go**

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

Notice that we have imported the dependency in the **learn.go** as well as this dependency is added in the **go.mod** file

```
"github.com/pborman/uuid"
```

Now let’s see try to remove this dependency completely from above module. Command **go mod tidy** will remove the dependency from **go.mod** file if it is not required in the source files. To illustrate this let’s remove the **learn.go** file that we created above. Now run the command

```
go mod tidy -v
```

It will give below output

```
unused github.com/pborman/uuid
```

Now examine the contents of **go.mod** file.  It will be as below

```
module learn

go 1.14
```

The

```
require github.com/pborman/uuid v1.2.1
```

line will be removed as it is not required in any of the source files. Also all entries of **[github.com](http://github.com)/pborman/uuid** and its depedencies will be removed from the **go.sum** file as well.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*