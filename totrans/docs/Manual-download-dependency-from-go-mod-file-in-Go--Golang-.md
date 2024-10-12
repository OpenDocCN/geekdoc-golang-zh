<!--yml
category: 未分类
date: 2024-10-13 06:29:18
-->

# Manual download dependency from go.mod file in Go (Golang)

> 来源：[https://golangbyexample.com/download-dependency-golang/](https://golangbyexample.com/download-dependency-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

Below command can be used to download the dependency that is present in the **go.mod** file

```
go mod download
```

This command is used to pre download all the dependency before the application is run. **go build** as well as **go install** also will download the dependencies and also build the binary. **go run** will also download and and run the binary as well. **go mod download** command is used when you want to pre download the dependencies without build or running it.

# **Example**

Let’s create a module first.

```
go mod init learn
```

Also let’s add direct dependency to the go.mod file too.

```
require github.com/pborman/uuid v1.2.1
```

With this dependency go.mod file will look like below.

```
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

Now we need to download the newly added dependency. Fo that we can use the below command

```
go mod download
```

The command will download the **github.com/pborman/uuid** module as well all its dependencies. Also it will update the **go.sum** file with the checksum and version of all direct and indirect dependencies. Now let’s check the **go.sum** file as well.

Do a cat **go.sum**

```
github.com/google/uuid v1.0.0 h1:b4Gk+7WdP/d3HZH8EJsZpvV7EtDOgaZLtnaNGIu1adA=
github.com/google/uuid v1.0.0/go.mod h1:TIyPZe4MgqvfeYDBFedMoGGpEw/LqOeaOT+nhxU+yHo=
github.com/pborman/uuid v1.2.1 h1:+ZZIw58t/ozdjRaXh/3awHfmWRbzYxJoAdNJxe/3pvw=
github.com/pborman/uuid v1.2.1/go.mod h1:X/NO0urCmaxf9VXbdlT7C2Yzkj2IKimNn4k+gtPdI/k=
```

**go.sum** file lists down the checksum of direct and indirect dependency required by the module.  [github.com](http://github.com)/google/uuid is internally used by the [github.com](http://github.com)/pborman/uuid . It is an indirect dependency of the module and hence it is recorded in the **go.sum** file.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*