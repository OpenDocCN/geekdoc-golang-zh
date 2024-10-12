<!--yml
category: 未分类
date: 2024-10-13 06:29:36
-->

# What does go mod tidy do in Go (Golang)

> 来源：[https://golangbyexample.com/go-mod-tidy/](https://golangbyexample.com/go-mod-tidy/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

This command will basically match the **go.mod** file with the dependencies required in the source files.

*   Download all the dependencies that are required in your source files and update **go.mod** file with that dependency.

*   Remove all dependencies from the go.mod file which are not required in the source files.

Below is the usage format for the command

```
go mod tidy [-v]
```

With **-v** flag, go mod tidy will print information of all the unused modules removed from **go.mod** file if any

# **Example**

Let’s see an an example. Create a module with import path as “**learn**“

```
go mod init learn
```

This command will create a **go.mod** file in the same directory. Since it is an empty module it doesn’t have any direct dependency specified yet. Let’s create a file named **uuid.go** in the same directory with the below contents

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

Now let’s see one more example where **go mod tidy** will remove the dependency from **go.mod** file if it is not required in the source files. To illustrate this let’s remove the **uuid.go** file that we created above. Now run the command

```
go mod tidy -v
```

It will give below output

```
unused github.com/pborman/uuid
```

Now examine the contents of **go.mod** file. It will be as below

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