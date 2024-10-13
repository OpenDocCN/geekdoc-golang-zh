<!--yml

category: 未分类

日期：2024-10-13 06:29:18

-->

# 从 go.mod 文件手动下载依赖项（Go）

> 来源：[`golangbyexample.com/download-dependency-golang/`](https://golangbyexample.com/download-dependency-golang/)

目录

+   概述

+   示例

# **概述**

可以使用下面的命令下载 **go.mod** 文件中存在的依赖项。

```go
go mod download
```

该命令用于在应用程序运行之前预下载所有依赖项。**go build** 和 **go install** 也将下载依赖项并构建二进制文件。**go run** 也将下载并运行二进制文件。**go mod download** 命令用于在不构建或运行的情况下预下载依赖项。

# **示例**

让我们先创建一个模块。

```go
go mod init learn
```

让我们也将直接依赖添加到 go.mod 文件中。

```go
require github.com/pborman/uuid v1.2.1
```

有了这个依赖，go.mod 文件将如下所示。

```go
module learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

现在我们需要下载新添加的依赖。为此，我们可以使用下面的命令

```go
go mod download
```

该命令将下载 **github.com/pborman/uuid** 模块以及它的所有依赖项。它还将更新 **go.sum** 文件，包含所有直接和间接依赖项的校验和和版本。现在让我们检查一下 **go.sum** 文件。

执行 `cat **go.sum**`

```go
github.com/google/uuid v1.0.0 h1:b4Gk+7WdP/d3HZH8EJsZpvV7EtDOgaZLtnaNGIu1adA=
github.com/google/uuid v1.0.0/go.mod h1:TIyPZe4MgqvfeYDBFedMoGGpEw/LqOeaOT+nhxU+yHo=
github.com/pborman/uuid v1.2.1 h1:+ZZIw58t/ozdjRaXh/3awHfmWRbzYxJoAdNJxe/3pvw=
github.com/pborman/uuid v1.2.1/go.mod h1:X/NO0urCmaxf9VXbdlT7C2Yzkj2IKimNn4k+gtPdI/k=
```

**go.sum** 文件列出了模块所需的直接和间接依赖项的校验和。[github.com](http://github.com)/google/uuid 被 [github.com](http://github.com)/pborman/uuid 内部使用。它是该模块的间接依赖，因此记录在 **go.sum** 文件中。


