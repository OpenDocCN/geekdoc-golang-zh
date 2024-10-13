<!--yml

分类：未分类

日期：2024-10-13 06:29:54

-->

# 理解 Go (Golang) 中的模块名称或模块导入路径

> 来源：[https://golangbyexample.com/module-import-path-golang/](https://golangbyexample.com/module-import-path-golang/)

目录

**   [概述](#Overview "Overview")

+   [该模块是一个实用模块，你计划将其发布到像 git 这样的版本控制系统(VCS)](#The_module_is_a_utility_module_and_you_plan_to_publish_your_module_to_VCS_like_git "该模块是一个实用模块，你计划将其发布到像 git 这样的版本控制系统(VCS)")

+   [该模块是一个实用模块，你不打算发布你的模块](#The_module_is_a_utility_module_and_you_dont_plan_to_publish_your_module "该模块是一个实用模块，你不打算发布你的模块")

+   [该模块是一个可执行模块](#The_module_is_a_executable_module "该模块是一个可执行模块")*  *# **概述**

模块是 Go 支持的依赖管理。模块的定义是包含相关包的集合，其根目录中有 **go.mod**。**go.mod** 文件定义了

+   模块导入路径。

+   模块成功构建所需的依赖要求。它定义了项目的依赖要求，并将其锁定到正确的版本。

现在让我们更详细地讨论模块导入路径。这是用于导入该模块所有包的前缀路径。让我们看看一个例子来更好地理解模块导入路径。为此，让我们创建一个模块

```go
go mod init sample.com/learn
```

上述命令将创建一个导入路径为 [sample.com](http://sample.com)/learn 的模块。它还会在同一目录中创建一个 **go.mod** 文件。让我们检查一下该文件的内容。执行 cat **go.mod**

```go
module sample.com/learn

go 1.14
```

go.mod 文件的第一行将包含模块导入路径，正如你在上面的 **go.mod** 文件中看到的那样。现在，这个模块导入路径有什么用。模块的导入路径用于从任何其他模块导入该模块的任何包。这个其他模块将使用模块导入路径作为前缀路径，加上包的文件夹路径。

```go
module_import_path + folder_path_of_package
```

因此，如果 **sample.com/learn** 模块包含位于目录 **math** 中的 **math** 包，那么任何其他模块都可以使用以下路径导入 **math** 包

```go
sample.com/learn/math
```

现在让我们看看决定模块导入路径命名的因素。可以有三种情况决定可以与模块一起使用的导入路径。

+   该模块是一个实用模块，你计划将其发布到 VCS 例如 git

+   该模块是一个实用模块，你不打算发布你的模块

+   该模块是一个可执行模块

# **该模块是一个实用模块，你计划将其发布到像 git 这样的版本控制系统(VCS)**

如果你计划发布你的模块，则模块名称应与托管该模块的仓库的 URL 匹配。Go 尝试使用模块的导入路径从 VCS 下载依赖项。

例如，注意该模块的导入路径 [https://github.com/pborman/uuid](https://github.com/pborman/uuid)。go.mod 文件如下所示

```go
module github.com/pborman/uuid

...
```

请注意，模块的URL与模块导入路径相同。

# **该模块是一个工具模块，您并不打算发布该模块。**

当您仅打算在本地使用工具模块时，就是这种情况。在这种情况下，导入路径可以是任何东西。参考这个示例，我们展示了如何导入本地模块。在这个示例中，我们为模块导入路径使用了一个不代表版本控制系统仓库的名称。

[https://golangbyexample.com/import-local-module-golang](https://golangbyexample.com/import-local-module-golang)/

# **该模块是一个可执行模块。**

在这种情况下，模块导入路径可以是任何东西。即使您计划将模块提交到版本控制系统，该导入路径也可以是非URL，因为它不会被其他模块使用。

然而，在创建模块时使用有意义的导入路径是一个好习惯。

+   [go](https://golangbyexample.com/tag/go/) * [golang](https://golangbyexample.com/tag/golang/) *
