<!--yml
category: 未分类
date: 2024-10-13 06:01:20
-->

# OOP: Polymorphism in Go Complete Guide

> 来源：[https://golangbyexample.com/oop-polymorphism-in-go-complete-guide/](https://golangbyexample.com/oop-polymorphism-in-go-complete-guide/)

Before understanding, polymorphism lets first understand what polymorphism is. 

“Same name many forms”

In the programming context, we also come across several use cases in which behavior is similar across different context. It is better for such behavior to be represented by the same name. This is where polymorphism comes into the picture in the programming context. From a programming perspective, there are two kinds of polymorphism possible-

*   Compile Time Polymorphism – In this type of polymorphism, the compiler is able to know which exact functions will be executed for a particular call. Some example of compile-time polymorphism is.  
    *   Function overloading – same function name having different arguments
    *   Operator overloading 
*   Run Time Polymorphism – In this type of polymorphism, the function to be called is decided at run time.

Let’s look at what type of Compile and Runtime Polymorphism is possible in GO

*   **[Compile Time Polymorphism in Go](https://golangbyexample.com/compile-time-polymorphism-go)**
*   **[Run Time Polymorphism in Go](https://golangbyexample.com/runtime-polymorphism-go/)**

**Conclusion: **

If you have read the above two articles then you will find out that compile-time polymorphism is not possible in GoLang. It only has Run-Time Polymorphism. Run-Time polymorphism is achieved in GO using interfaces.