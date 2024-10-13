<!--yml

类别：未分类

日期：2024-10-13 06:11:46

-->

# 理解 MAC 上的 /etc/paths 文件和 /etc/paths.d 目录

> 来源：[`golangbyexample.com/understand-etc-paths-pathsd-mac/`](https://golangbyexample.com/understand-etc-paths-pathsd-mac/)

MAC OS 使用 **/etc/paths** 文件和 **/etc/paths.d** 目录来设置 **PATH** 环境变量的值。一个工具 **path_helper** 用于根据 **/etc/paths** 文件的内容和 **/etc/paths.d** 目录中文件的内容来设置 **PATH** 环境变量。这就是如何运行 path_helper。

```go
eval `/usr/libexec/path_helper -s`
```

让我们看看这两者在设置路径时的作用。

**/etc/paths 文件**

该文件包含需要设置在 **PATH** 环境变量中的路径。如果我在 MAC 上打印该文件的内容，它输出

```go
/ $ cat /etc/paths
/usr/local/bin
/usr/bin
/bin
/usr/sbin
/sbin
```

上述所有路径都被添加到 PATH 变量中。

**/etc/paths.d 目录**

此目录包含文件列表。每个文件包含需要添加到 PATH 变量的路径。作为演示，如果我使用这里提供的 GO 的 .pkg 安装程序在我的 MAC 上安装 GO – [`golang.org/dl/`](https://golang.org/dl/)。

安装后，它将在 **‘/etc/paths.d’** 目录中创建一个名为 **‘go’** 的文件。让我们输出该文件的内容。

```go
/ $ cat /etc/paths.d/go
/usr/local/go/bin
```

它输出‘/usr/local/go/bin’，该路径将被添加到 PATH 环境变量中。

**注意事项：**

使用 **‘/etc/paths.d’** 目录的主要原因是 **‘/etc/paths’** 会在系统升级时被修改和/或替换，而 **‘/etc/paths.d’** 目录的内容不会受到这种变化的影响。
