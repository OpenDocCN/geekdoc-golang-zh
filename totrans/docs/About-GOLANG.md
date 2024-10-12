<!--yml
category: 未分类
date: 2024-10-13 06:31:13
-->

# About GOLANG

> 来源：[https://golangbyexample.com/about-golang/](https://golangbyexample.com/about-golang/)

This is chapter 1 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

Below is the table of contents for the current tutorial.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [History](#History "History")
*   [What makes GO different](#What_makes_GO_different "What makes GO different")
    *   [Simple to Use Syntax](#Simple_to_Use_Syntax "Simple to Use Syntax")
    *   [Compiled Language with fast Compilation](#Compiled_Language_with_fast_Compilation "Compiled Language with fast Compilation")
    *   [Static Type Language](#Static_Type_Language "Static Type Language")
    *   [Garbage Collected](#Garbage_Collected "Garbage Collected")
    *   [Built In Concurrency](#Built_In_Concurrency "Built In Concurrency")
    *   [Standard Library](#Standard_Library "Standard Library")
    *   [Tooling](#Tooling "Tooling")
*   [Comparison with other programming languages](#Comparison_with_other_programming_languages "Comparison with other programming languages")
*   [Disadvantages of GO](#Disadvantages_of_GO "Disadvantages of GO")
    *   [No inheritance in GO](#No_inheritance_in_GO "No inheritance in GO")
    *   [No Generics in GO](#No_Generics_in_GO "No Generics in GO")
    *   [No Compile Time Polymorphism or Function Overloading](#No_Compile_Time_Polymorphism_or_Function_Overloading "No Compile Time Polymorphism or Function Overloading")
*   [Conclusion:](#Conclusion "Conclusion:")*  *# **Overview**

This is the first tutorial that will give you an introduction to the GOLANG language. Go is a statically typed, compiled programming language. It is an open-source language and was developed at Google by

*   Robert Griesemer
*   [Rob Pike](https://en.wikipedia.org/wiki/Rob_Pike)
*   [Ken Thompson](https://en.wikipedia.org/wiki/Ken_Thompson)

Since GO is an open-source programming language, it is hosted on Github and  is open for contribution by anyone.

[https://github.com/golang/go](https://github.com/golang/go)

Go has C style syntax but provides memory safety, garbage collection and inbuilt concurrency.

# **History**

GO had its first stable release in 2011.

# **What makes GO different**

Let’s see some of the things which makes go different from other programming languages.

## **Simple to Use Syntax**

Go is syntactically similar to C and has a really simple syntax. It is easy to learn with little facets. It is not overwhelmed with features and thus it is easier to write readable and maintainable code in GO. Also, GO has very few keywords compared to other mainstream languages. The GO specification mentions 25 keywords.

[https://golang.org/ref/spec#Keywords](https://golang.org/ref/spec#Keywords)

## **Compiled Language with fast Compilation**

Go compiles into a native executable. Its compilation is really fast and it is attributed due to a couple of reasons. You can check out this link for more details related to why GO compiles fast. When you build a GO program the resulting binary is what it is. There are no other dependencies. You can port this binary to any platform which supports GO and it can be executed there. This is different when you compare it with other programming languages such as JAVA, Python, where to create a self-contained binary there are a bunch of dependencies that need to be taken care of and packed along. There is no concept of Dynamic Link Library in GO in fact it supports static linking. GO is statically linked with all the libraries into a single standalone big fat binary.

## **Static Type Language**

Even though GO has a really neat syntax, it is a strong and static type language. There is no implicit conversion allowed in the language. Since it is the static type most of the errors related to mismatched type are caught during compile time unlike languages like Python, Ruby. etc

## **Garbage Collected**

Go is a garbage-collected language. That means you don’t have to worry about freeing up memory. So you don’t have to be bothered about malloc() and free() statement like in C, C++

## **Built In Concurrency**

GO through its inbuilt concurrency tries to solve two major problems associated with other languages

*   The creation of threads is not memory efficient.

*   Communication between threads is difficult with problems such as thread locking, deadlock, etc.

Go approach to concurrency can be best described by

```
Do not communicate by sharing memory; instead, share memory by communicating
```

GO tries to achieve the above through its two concurrency primitives

*   **Goroutine** – lightweight independent execution to achieve concurrency/parallelism. Its size starts at 8kb as compared to the JAVA thread whose size is around 1 MB.

*   **Channels** – provides synchronization and communication between goroutines.

## **Standard Library**

GO standard library is rich in features and support for almost everything which is needed for building all types of applications. There are also tons of packages which have been created by the GO community on top of GO standard library which is ready for use and available on Github

## **Tooling**

GO support for tooling is strong and rich. It has tools for code formatting, unit testing, race detector, linting, memory analysis tools, tools for generating documentation  etc.

# **Comparison with other programming languages**

With all the above advantages if you compare with other programming languages you will notice

**With Java**

*   GO is as fast as JAVA but with its simpler syntax it is easier to learn, write and maintain GO code as compared to JAVA

**With C/C++**

*   GO is a little less fast than C but again with its simpler syntax it is easier to learn, write and maintain GO code as compared to C/C++

*   GO is also safer than C/C++ as it is a garbage collected language and also doesn’t allow nasty pointer operations present in C/C++. Being a GC language you don’t have to worry about malloc() and free() operations

**With Python/Ruby/Java Script**

*   GO is very fast as compared to the above programming languages

*   The code readability part is easier in these languages but GO is a compiled and static type language that helps catch some nasty bugs in the compilation phase itself.

# **Disadvantages of GO**

Let’s see some of the disadvantages of go as well

## **No inheritance in GO**

Go doesn’t have classes. It has structs. Even though the functionality of structs of pretty similar to classes but GO favors composition over inheritance. GO also doesn’t support type hierarchy. It doesn’t have keywords such as extends or implements as in JAVA. So in that terms GO is not a pure OOP language. The missing of such features might seem odd to coders who all well versed in coding in an OOP manner.

## **No Generics in GO**

Due to the absence of generics in GO, it expects you to write a very explicit code for what you want to do. Although there are talks of adding Generics support to GO very soon.

## **No Compile Time Polymorphism or Function Overloading**

Function Overloading is not possible in GO. See this faq for the reason [https://golang.org/doc/faq#overloading](https://golang.org/doc/faq#overloading)

According to the above faq, things are simpler without it. Although there are some workarounds to achieve the same. Can refer to this link

[https://golangbyexample.com/function-method-overloading-golang/](https://golangbyexample.com/function-method-overloading-golang/)

# **Conclusion:**

To summarize what we have discussed so far

*   Go is static typing with performance and runtime efficiency matching C/C++

*   Really simple syntax and easy to write code as in Python, Ruby

*   Inbuilt concurrency with a good standard library and rich tooling.

*   GO is not a pure OOP language with a different style of error handling as compared to other mainstream languages

This is the basic introduction to golang. Hope you have liked this article. Please share feedback or improvements or mistakes in the comments.

****Next Tutorial –**** [GO Installation](https://golangbyexample.com/golang-installation)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*