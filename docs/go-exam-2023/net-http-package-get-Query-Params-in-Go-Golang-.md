<!--yml

类别：未分类

日期：2024-10-13 06:02:36

-->

# net/http 包获取 Go (Golang) 中的查询参数

> 来源：[`golangbyexample.com/net-http-package-get-query-params-golang/`](https://golangbyexample.com/net-http-package-get-query-params-golang/)

**注意：** 如果你对学习 Golang 感兴趣，我们有一个全面的 Golang 教程系列。请务必查看一下 – [Golang 综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)。现在让我们来看当前的教程。

在 HTTP 请求的上下文中，通常需要获取作为请求一部分发送的查询参数。一个特定的查询参数键值可以有一个或多个值。

+   在下面的示例中，**filters** 有一个值，即 [“color”]。

```go
http://localhost:8080/products?filters=color
```

+   在下面的示例中，**filters** 有多个值，即 [“color”, “price”, “brand”]。注意如何定义多个值。

```go
http://localhost:8080/products?filters=red&filters=color&filters=price&filters=brand
```

让我们探索获取这些查询参数的两种方法。

# 1.**使用 reqeust.URL.Query()**

查询参数位于 **URL** 本身。我们通过 r.URL.Query() 获取查询参数，该函数返回 **Values**，它是 map[string][]string。存在两种情况：

**1.1 当特定键在查询参数中包含多个值时。例如，请参见下面的请求**

```go
http://localhost:8080/products?filters=red&filters=color&filters=price&filters=brand
```

**代码：**

```go
package main

import (
    "fmt"
    "net/http"
    "strings"
)

func main() {
    getProductsHandler := http.HandlerFunc(getProducts)
    http.Handle("/products", getProductsHandler)
    http.ListenAndServe(":8080", nil)
}

func getProducts(w http.ResponseWriter, r *http.Request) {
    query := r.URL.Query()
    filters, present := query["filters"] //filters=["color", "price", "brand"]
    if !present || len(filters) == 0 {
        fmt.Println("filters not present")
    }
    w.WriteHeader(200)
    w.Write([]byte(strings.Join(filters, ",")))
}
```

**输出：**

```go
color,price,brand 
```

**1.2 当特定键在查询参数中包含单个值时。例如，请参见下面的请求**

```go
http://localhost:8080/products?filters=color
```

当我们知道特定键在查询参数中只有一个值时，可以使用 r.URL.Query().Get(keyName)。Get 函数将获取与该键关联的第一个值。如果你想获取所有值，则必须直接访问映射，如我们在上面的程序中所做的。在下面的程序中，我们在 r.URL.Query() 上使用 Get()，并且它返回一个单一值。

```go
package main

import (
    "net/http"
)

func main() {
    getProductsHandler := http.HandlerFunc(getProducts)
    http.Handle("/products", getProductsHandler)
    http.ListenAndServe(":8080", nil)
}

func getProducts(w http.ResponseWriter, r *http.Request) {
    query := r.URL.Query()
    filters := query.Get("filters") //filters="color"
    w.WriteHeader(200)
    w.Write([]byte(filters))
}
```

**输出：**

```go
color
```

# **2.使用 request.Form**

查询参数位于 **URL** 本身。我们通过 r.URL.Query() 获取查询参数，该函数返回 **values**，它是 map[string][]string。存在两种情况：

**2.1** **当特定键在查询参数中包含多个值时。例如，请参见下面的请求**

```go
http://localhost:8080/products?filters=red&filters=color&filters=price&filters=brand
```

**代码：**

```go
package main

import (
    "fmt"
    "net/http"
    "strings"
)

func main() {
    getProductsHandler := http.HandlerFunc(getProducts)
    http.Handle("/products", getProductsHandler)
    http.ListenAndServe(":8080", nil)
}

func getProducts(w http.ResponseWriter, r *http.Request) {
    r.ParseForm()
    filters, present := r.Form["filters"] //filters=["color", "price", "brand"]
    if !present || len(filters) == 0 {
        fmt.Println("filters not present")
    }
    w.WriteHeader(200)
    w.Write([]byte(strings.Join(filters, ",")))
}
```

**输出：**

```go
color,price,brand
```

需要注意的一个重要点：

+   在尝试通过 r.Form 获取查询参数值时，要小心在 POST 和 PUT 请求中，主体参数会优先于 URL 查询字符串值。即，如果有一个键 **X** 同时出现在表单主体（=“a”）和查询参数（=“b”）中。那么调用 r.Form[“X”] 时，它将返回 [“a”] 而不是 [“b”]。

**2.2 当特定键在查询参数中包含单个值时。例如，请参见下面的请求**

```go
http://localhost:8080/products?filters=color
```

当我们知道特定键在查询参数中只有一个值时，可以使用 r.FormValue(keyName)。FormValue 函数将获取与该键关联的第一个值。如果你想获取所有值，则必须直接访问请求的表单映射，正如我们在上面的程序中所做的。在下面的程序中，我们使用 FormValue() 函数，并且它返回一个单一值。

```go
package main

import (
    "net/http"
)

func main() {
    getProductsHandler := http.HandlerFunc(getProducts)
    http.Handle("/products", getProductsHandler)
    http.ListenAndServe(":8080", nil)
}

func getProducts(w http.ResponseWriter, r *http.Request) {
    filters := r.FormValue("filters") //filters=["color"]
    w.WriteHeader(200)
    w.Write([]byte(filters))
}
```

**输出：**

```go
color
```

需要注意的一个重要点：

+   在通过 `r.FormValue()` 获取查询参数值时，要注意在 POST 和 PUT 请求的情况下，主体参数会优先于 URL 查询字符串中的值，也就是说，如果有一个键 **X** 同时出现在表单主体（=“a”）和查询参数（=“b”）中。那么在调用 `r.Form[“X”]` 时，它将返回 [“a”] 而不是 [“b”]。


