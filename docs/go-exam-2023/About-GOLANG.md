<!--yml

类别：未分类

日期：2024-10-13 06:31:13

-->

# 关于 GOLANG

> 来源：[https://golangbyexample.com/about-golang/](https://golangbyexample.com/about-golang/)

这是 golang 综合教程系列的第一章。有关系列其他章节，请参阅此链接 – [Golang 综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

以下是当前教程的目录。

目录

**   [概述](#Overview "概述")

+   [历史](#History "历史")

+   [GO 的不同之处](#What_makes_GO_different "GO 的不同之处")

    +   [简单易用的语法](#Simple_to_Use_Syntax "简单易用的语法")

    +   [快速编译的编译语言](#Compiled_Language_with_fast_Compilation "快速编译的编译语言")

    +   [静态类型语言](#Static_Type_Language "静态类型语言")

    +   [垃圾回收](#Garbage_Collected "垃圾回收")

    +   [内置并发](#Built_In_Concurrency "内置并发")

    +   [标准库](#Standard_Library "标准库")

    +   [工具](#Tooling "工具")

+   [与其他编程语言的比较](#Comparison_with_other_programming_languages "与其他编程语言的比较")

+   [GO 的缺点](#Disadvantages_of_GO "GO 的缺点")

    +   [GO 中没有继承](#No_inheritance_in_GO "GO 中没有继承")

    +   [GO 中没有泛型](#No_Generics_in_GO "GO 中没有泛型")

    +   [没有编译时多态或函数重载](#No_Compile_Time_Polymorphism_or_Function_Overloading "没有编译时多态或函数重载")

+   [结论：](#Conclusion "结论：")*  *# **概述**

这是第一篇将为你介绍 GOLANG 语言的教程。Go 是一种静态类型的编译编程语言。它是一种开源语言，由谷歌开发。

+   罗伯特·格里泽默

+   [罗布·派克](https://en.wikipedia.org/wiki/Rob_Pike)

+   [肯·汤普逊](https://en.wikipedia.org/wiki/Ken_Thompson)

由于 GO 是一种开源编程语言，它托管在 GitHub 上，并且任何人都可以贡献代码。

[https://github.com/golang/go](https://github.com/golang/go)

Go 具有 C 风格的语法，但提供内存安全、垃圾回收和内置并发。

# **历史**

GO 于 2011 年首次稳定发布。

# **GO 的不同之处**

让我们看看一些使 Go 与其他编程语言不同的特点。

## **简单易用的语法**

Go 的语法与 C 相似，语法非常简单。学习起来容易，几乎没有复杂的方面。它的特性不算繁杂，因此编写可读和可维护的代码变得更容易。此外，与其他主流语言相比，GO 的关键字非常少，GO 规范提到有 25 个关键字。

[https://golang.org/ref/spec#Keywords](https://golang.org/ref/spec#Keywords)

## **快速编译的编译语言**

Go 编译成本地可执行文件。它的编译速度非常快，这归因于几个原因。您可以查看这个链接以获取有关 Go 编译速度快的更多细节。当您构建一个 Go 程序时，生成的二进制文件就是它的本身。没有其他依赖项。您可以将这个二进制文件移植到任何支持 Go 的平台上，并在那执行。这与其他编程语言如 JAVA、Python 的情况不同，因为创建一个自包含的二进制文件时需要处理和打包一堆依赖项。在 Go 中没有动态链接库的概念，实际上它支持静态链接。Go 与所有库静态链接成一个单独的庞大二进制文件。

## **静态类型语言**

尽管 Go 具有非常简洁的语法，但它是一种强大且静态的类型语言。该语言不允许隐式转换。由于是静态类型，绝大多数与类型不匹配相关的错误在编译时就会被捕获，不像 Python、Ruby 等语言。

## **垃圾回收**

Go 是一种垃圾回收语言。这意味着您不必担心释放内存。因此，您不必像在 C、C++ 中那样关心 malloc() 和 free() 语句。

## **内置并发**

Go 通过其内置并发试图解决与其他语言相关的两个主要问题。

+   线程的创建并不高效地使用内存。

+   线程之间的通信较为困难，存在线程锁定、死锁等问题。

Go 对并发的处理可以最好地描述为。

```
Do not communicate by sharing memory; instead, share memory by communicating
```

Go 通过其两种并发原语来实现上述目标。

+   **Goroutine** – 轻量级独立执行以实现并发/并行。其大小从 8kb 开始，而 JAVA 线程的大小约为 1 MB。

+   **通道** – 提供 goroutine 之间的同步和通信。

## **标准库**

Go 的标准库功能丰富，支持几乎所有构建各种类型应用所需的功能。Go 社区还在 Go 标准库的基础上创建了大量现成可用的包，并可在 Github 上获取。

## **工具**

Go 对工具的支持强大且丰富。它有用于代码格式化、单元测试、竞争检测、代码检查、内存分析工具、生成文档的工具等。

# **与其他编程语言的比较**

如果将上述所有优点与其他编程语言进行比较，您会注意到。

**与 Java 的比较**

+   Go 的速度与 JAVA 相当，但由于其更简单的语法，与 JAVA 相比，学习、编写和维护 Go 代码更容易。

**与 C/C++ 的比较**

+   Go 的速度比 C 稍慢，但由于其简化的语法，与 C/C++ 相比，学习、编写和维护 Go 代码更容易。

+   GO也比C/C++更安全，因为它是垃圾收集语言，也不允许存在于C/C++中的棘手指针操作。作为一门GC语言，你无需担心malloc()和free()操作。

**使用 Python/Ruby/Java Script**

+   与上述编程语言相比，GO非常快速

+   这些语言的代码可读性更强，但GO是编译型和静态类型的语言，有助于在编译阶段捕获一些棘手的错误。

# **GO的缺点**

让我们看看GO的一些缺点

## **GO中没有继承**

Go没有类，只有结构体。尽管结构体的功能与类非常相似，但GO更倾向于组合而非继承。GO也不支持类型层次结构，没有像JAVA中的extends或implements这样的关键字。因此在这方面，GO不是一种纯粹的面向对象编程语言。缺少这些特性可能对习惯于面向对象编程的编码者来说显得奇怪。

## **GO中没有泛型**

由于GO中缺少泛型，它期望你为想要做的事情编写非常明确的代码。尽管有关于很快会向GO添加泛型支持的讨论。

## **没有编译时多态或函数重载**

在GO中不可能进行函数重载。请查看这个常见问题了解原因 [https://golang.org/doc/faq#overloading](https://golang.org/doc/faq#overloading)

根据上述常见问题，没有这些东西会简单得多。尽管有一些变通方法可以实现相同的功能。可以参考这个链接

[https://golangbyexample.com/function-method-overloading-golang/](https://golangbyexample.com/function-method-overloading-golang/)

# **结论：**

总结一下我们到目前为止讨论的内容

+   Go是静态类型，其性能和运行时效率与C/C++相当

+   语法非常简单，写代码像在Python、Ruby中一样容易

+   内置的并发支持，拥有良好的标准库和丰富的工具。

+   GO不是一种纯粹的面向对象编程语言，与其他主流语言相比，其错误处理风格不同

这是对Golang的基本介绍。希望你喜欢这篇文章。请在评论中分享反馈、改进意见或错误。

****下一个教程 –**** [GO 安装](https://golangbyexample.com/golang-installation)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
