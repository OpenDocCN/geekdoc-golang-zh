<!--yml
category: 未分类
date: 2024-10-13 06:28:02
-->

# Declaring a constant in Go (Golang)

> 来源：[https://golangbyexample.com/declaring-constant-go/](https://golangbyexample.com/declaring-constant-go/)

A constant is anything that doesn’t change its value. In Go const can be either of type string, numeric, boolean, and characters.

A constant can be declared using the **const** keyword. An important point to be noted is that the value has to be assigned while declaring a constant. It is unlike variables where value can be assigned later.

*   Declaring a **const** along with specifying the type – It starts with a **const** keyword, followed by the name and then the type. The value must also be assigned right away as discussed above

```
const c string = "circle"
```

*   Declaring a const without specifying type – A const declared without type is an untyped constant. We will learn more about typed and untyped constant later on. For now, it’s important to know that const declared without type has a default hidden type. The constant will be given a type once it is assigned to a variable in any way (direct initialization, passing to a function, etc).

```
const c = "circle"
```

*   Declaring multiple const together

```
const (
  c = "circle"
  s = "square"
)
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)