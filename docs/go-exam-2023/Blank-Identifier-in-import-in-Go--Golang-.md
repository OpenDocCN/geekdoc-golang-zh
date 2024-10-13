<!--yml

类别：未分类

日期：2024-10-13 06:30:44。

-->

# Go (Golang) 中的空标识符导入。

> 来源：[https://golangbyexample.com/blank-identifier-import-golang/](https://golangbyexample.com/blank-identifier-import-golang/)

导入包中的空标识符意味着为导入的包指定空导入。其语法为：

```go
import _ <directory_path></directory_path>
```

什么是这个空导入，为什么要使用它。为此你需要理解两件事：

1.  关于 **init** 函数。

1.  关于由下划线（‘**_**‘）表示的空标识符。

**init() 函数**。

init() 函数是一个特殊的函数，用于初始化包的全局变量。这些函数在包初始化时执行。包中的每个 GO 源文件可以有自己的 init() 函数。每当你在程序中导入任何包时，程序执行时，将首先调用属于该导入包的 GO 源文件中的 init 函数（如果存在）。关于 init 函数需要注意的一些要点：

+   Init 函数是可选的。

+   Init 函数不接受任何参数。

+   Init 函数没有任何返回值。

+   Init 函数是隐式调用的。由于它是隐式调用的，init 函数无法从任何地方引用。

+   在同一个源文件中可以有多个 init() 函数。

**关于空标识符**。

Go 不允许任何未使用的变量。任何未使用的变量可以被空标识符（‘_’）替代。

所以现在，当使用一个包的空导入时，

+   当前程序中未使用导入的包。

+   但我们打算导入该包，以便可以正确调用属于该包的 GO 源文件中的 init 函数，并进行变量的初始化。

所以基本上，当一个包仅仅为了其副作用被导入时，就使用空导入。

例如，mysql 包作为空导入，用于其副作用，即在 mysql 包的 init() 函数中注册 mysql 驱动程序作为数据库驱动程序，而不导入任何其他函数：

```go
_ "github.com/go-sql-driver/mysql"
```

要使用任何 mysql 库，例如 gorm 或 xorm，上述空导入是必要的。

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/)。
