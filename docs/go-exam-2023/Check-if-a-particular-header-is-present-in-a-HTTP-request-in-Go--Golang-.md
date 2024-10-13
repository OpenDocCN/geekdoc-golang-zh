<!--yml

类别：未分类

日期：2024-10-13 06:31:33

-->

# 检查 Go (Golang) 中 HTTP 请求是否存在特定 header。

> 来源：[`golangbyexample.com/header-present-http-golang/`](https://golangbyexample.com/header-present-http-golang/)

目录

+   概述

# **概述**

以下是 header 在 Go 中表示的格式。

```go
type Header map[string][]string
```

所以在内部，header 是一个键值映射。

+   键以规范形式表示。规范形式意味着第一个字符和连字符后面的任何字符都是大写字母。其余字符都是小写字母。规范形式的示例包括：

```go
Content-Type
Accept-Encoding
```

+   值表示为字符串数组。为什么是字符串数组？因为在请求中使用相同键的两个 header 和不同值是完全可以的。两个值将被收集到数组中。

例如，如果在传入请求中有以下 headers：

```go
content-type: applcation/json
foo: bar1
foo: bar2
```

然后在服务器上，header 将如下所示：

```go
map[string][]string {
  "Content-Type": {"application/json"},
  "Foo": {"bar1" , "bar2"}
}
```

由于 header 是一个映射，我们可以利用映射的特性检查某个特定键是否存在。以下是检查映射中键是否存在的格式。

```go
val, ok := mapName[key]
```

这里有两种情况

+   如果键存在，**val** 变量将是映射中键的值，而 **ok** 变量将为 true。

+   如果键不存在，**val** 变量将是值类型的默认零值，而 **ok** 变量将为 false。

让我们来看一个例子。

```go
package main

import (
    "fmt"
    "net/http"
)

func main() {
    handler := http.HandlerFunc(handleRequest)
    http.Handle("/example", handler)
    http.ListenAndServe(":8080", nil)
}

func handleRequest(w http.ResponseWriter, r *http.Request) {
    headers := r.Header
    val, ok := headers["Content-Type"]
    if ok {
        fmt.Printf("Content-Type key header is present with value %s\n", val)
    } else {
        fmt.Println("Content-Type key header is not present")
    }
    val, ok = headers["someKey"]
    if ok {
        fmt.Printf("someKey key header is present with value %s\n", val)
    } else {
        fmt.Println("someKey key header is not present")
    }
}
```

在上述程序中，我们启动了一个监听端口 8080 的服务器。我们还在该端点定义了一个 URL。运行此服务器并进行以下 API 调用。

```go
curl -v -X POST http://localhost:8080/example -H "content-type: application/json" 
```

运行此 API 后，请检查终端中的输出。它将输出结果。你可以检查输出。content-type header 存在，因此输出其值。someKey header 不存在，因此打印未找到。此外，请注意，在访问 header 映射时，我们需要以规范形式输入键。例如，即使 curl 中提供的 header 是：

```go
content-type
```

但是在代码中访问 header 映射时，我们使用的键是规范形式 **Content-Type**。

```go
val, ok := headers["Content-Type"]
```

如果我们只想检查一个键是否存在而不需要值，则可以用空标识符即“_”替代值。

```go
_, ok = employeeSalary["Sam"]
```


