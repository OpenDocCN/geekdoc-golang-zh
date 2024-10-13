<!--yml

类别：未分类

日期：2024-10-13 06:01:20

-->

# OOP：Go中的多态完全指南

> 来源：[https://golangbyexample.com/oop-polymorphism-in-go-complete-guide/](https://golangbyexample.com/oop-polymorphism-in-go-complete-guide/)

在理解多态之前，首先了解什么是多态。

“同名多形态”

在编程上下文中，我们还会遇到许多行为在不同环境中相似的用例。将此类行为用相同的名称表示更为合适。这就是多态在编程中的意义。从编程的角度来看，有两种可能的多态性——

+   编译时多态——在这种多态中，编译器能够知道特定调用将执行哪些具体函数。编译时多态的示例包括：

    +   函数重载——同一函数名具有不同的参数。

    +   运算符重载

+   运行时多态——在这种多态中，调用的函数在运行时决定。

让我们看看在GO中可能实现的编译时和运行时多态的类型。

+   **[Go中的编译时多态](https://golangbyexample.com/compile-time-polymorphism-go)**

+   **[Go中的运行时多态](https://golangbyexample.com/runtime-polymorphism-go/)**

**结论：**

如果你阅读了以上两篇文章，你会发现GoLang中不支持编译时多态。它只具有运行时多态。运行时多态在GO中是通过接口实现的。
