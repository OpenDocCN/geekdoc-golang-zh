<!--yml
category: 未分类
date: 2024-10-13 06:04:52
-->

# Difference between method and function in GO

> 来源：[https://golangbyexample.com/difference-between-method-function-go/](https://golangbyexample.com/difference-between-method-function-go/)

There are some important differences between method and function. Let’s see the signature of both

**Function:**

```
func some_func_name(arguments) return_values
```

**Method:**

```
func (receiver receiver_type) some_func_name(arguments) return_values
```

From the above signature, it is clear that method has a receiver argument. A receiver can be a struct or any other type. The method will have access to the properties of the receiver and can call the receiver’s other methods.
This is the only difference between function and method, but due to it they differ in terms of functionality they offer

*   A function can be used as first-order objects and can be passed around while methods cannot.
*   Methods can be used for chaining on the receiver while function cannot be used for the same.
*   There can exist different methods with the same name with a different receiver, but there cannot exist two different functions with the same name in the same package.

*   [function](https://golangbyexample.com/tag/function/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [method](https://golangbyexample.com/tag/method/)