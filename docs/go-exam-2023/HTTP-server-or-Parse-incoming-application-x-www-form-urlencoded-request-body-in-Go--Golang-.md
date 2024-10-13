<!--yml

类别：未分类

日期：2024-10-13 06:32:28

-->

# HTTP 服务器或在 Go（Golang）中解析传入的 application/x-www-form-urlencoded 请求主体。

> 来源：[`golangbyexample.com/url-encoded-body-golang/`](https://golangbyexample.com/url-encoded-body-golang/)

**注意**：这篇文章在服务器端解析**application/x-www-form-urlencoded**请求。如果你使用 HTTP 客户端并尝试发送**application/x-www-form-urlencoded**请求，请查看以下链接。

[`golangbyexample.com/http-client-urlencoded-body-go`](https://golangbyexample.com/http-client-urlencoded-body-go)/

目录

+   概述

+   示例

# **概述**

**application/x-www-form-urlencoded**内容类型的请求主体就像一个巨大的查询字符串。类似于 URI 中的查询字符串，它是一个具有以下格式的键值对。

```go
key1=value1&key2=value21&key2=value22&key3=value3
```

其中有以下键值对。

+   key1, value1

+   key2 有两个值，即 value21 和 value22。

+   key3, value3

每个键值对由**&**分隔，如果同一键有多个值，则会有两个该键值对的条目。此外，每个键和值都经过 URL 编码，类似于查询字符串。

现在可能会出现的问题是，如果**x-www-form-urlencoded**就像查询字符串，那么它为何存在。原因是查询字符串是 URI 的一部分，而 URI 的长度有限，因此你只能在查询字符串中发送有限数量的键值对。虽然**x-www-form-urlencoded**请求主体的长度没有限制，但它受到服务器允许的最大请求主体大小的限制，通常大多数服务器为 10MB。现在，让我们看看如何在 golang 中解析**x-www-form-urlencoded**。

# **示例**

net/http 包的请求对象有两个字段可以保存表单数据。[`golang.org/src/net/http/request.go`](https://golang.org/src/net/http/request.go)

这两个字段是。

+   **Form** – 它包含查询字符串和**x-www-form-urlencoded**请求主体的组合数据。

+   **PostForm**– 它仅包含**x-www-form-urlencoded**请求主体。

要获取**x-www-form-urlencoded**请求主体，我们需要首先在请求对象上调用以下函数。

```go
request.ParseForm()
```

此函数基本上执行以下操作。

+   它解析 URL 中存在的查询字符串，并填充请求对象的**Form**字段。

```go
request.Form
```

+   之后，如果请求方法是 PUT、POST 或 PATCH，且请求主体的内容类型是 x-www-form-urlencoded，则它也会解析请求主体，并填充请求对象的上述两个字段。

```go
request.Form
request.PostForm
```

+   如果请求主体的内容类型不是 x-www-form-urlencoded，或者请求方法不是 PUT、POST 或 PATCH，则**request.PostForm**将初始化为非空的空值。

+   主体参数优先于 URL 查询字符串值，即如果在表单主体和查询参数中都有一个键，则表单主体中的值将优先。

+   此外，请注意这个函数是幂等的。

**Form** 和 **PostForm** 字段都是以下类型

```go
type Values map[string][]string
```

这是一个地图，其中

+   键是一个字符串

+   值是一个字符串数组。它是字符串数组，因为可能有多个值。

因此，基本上为了获取 x-www-url-encoded 请求体，我们需要先在请求对象上调用 **ParseForm** 函数。这将填充 **Form** 和 **PostForm** 字段。然后我们可以访问这些字段以获取 **x-www-form-urlencoded** 请求体。例如，假设我们有以下 **x-www-form-urlencoded** 请求体。

```go
name=John
age=21
hobbies=sports
hobbies=music
```

然后你可以像下面这样访问 **name** 字段

```go
request.Form["name"]
request.PostForm["name"]
```

两者都会返回

```go
["John"]
```

它是一个数组，因为键可以有多个值。

此外，请求对象还定义了一个函数 **FormValue**，可用于获取与键关联的第一个值。它仅返回第一个值，而不是数组。例如

```go
request.FormValue("hobbies")
```

将返回

```go
sports
```

让我们看看一个程序

```go
package main

import (
	"fmt"
	"net/http"
)

type employee struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}

func main() {
	createEmployeeHanlder := http.HandlerFunc(createEmployee)
	http.Handle("/employee", createEmployeeHanlder)
	http.ListenAndServe(":8080", nil)
}

func createEmployee(w http.ResponseWriter, r *http.Request) {
	headerContentTtype := r.Header.Get("Content-Type")
	if headerContentTtype != "application/x-www-form-urlencoded" {
		w.WriteHeader(http.StatusUnsupportedMediaType)
		return
	}
	r.ParseForm()
	fmt.Println("request.Form::")
	for key, value := range r.Form {
		fmt.Printf("Key:%s, Value:%s\n", key, value)
	}
	fmt.Println("\nrequest.PostForm::")
	for key, value := range r.PostForm {
		fmt.Printf("Key:%s, Value:%s\n", key, value)
	}

	fmt.Printf("\nName field in Form:%s\n", r.Form["name"])
	fmt.Printf("\nName field in PostForm:%s\n", r.PostForm["name"])
	fmt.Printf("\nHobbies field in FormValue:%s\n", r.FormValue("hobbies"))

	w.WriteHeader(200)
	return
}
```

在上述程序中，我们在端口 **8080** 上运行一个服务器。同时注意，我们首先调用了 **ParseForm** 函数，然后访问请求对象的 **Form** 和 **PostForm** 字段。

另外，看看我们如何在 **Form** 和 **PostForm** 中访问 **name** 字段的值。

```go
r.Form["name"]
r.PostForm["name"]
```

此外，我们还在请求对象上调用 **FormValue** 函数以访问 **hobbies** 字段，如下所示。

```go
r.FormValue("hobbies")
```

运行上述程序。它将在端口 **8080** 启动一个服务器。

现在让我们尝试一些 curl 调用。

+   仅传递 x-www-form-urlencoded 请求体。没有查询字符串。

```go
curl -v -X POST 'http://localhost:8080/employee' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'name=John' \
--data-urlencode 'age=18' \
--data-urlencode 'hobbies=sports' \
--data-urlencode 'hobbies=music'
```

**输出**

```go
request.Form:: 
Key:name, Value:[John] 
Key:age, Value:[18] 
Key:hobbies, Value:[sports music] 

request.PostForm:: 
Key:name, Value:[John] 
Key:age, Value:[18] 
Key:hobbies, Value:[sports music] 

Name field in Form:[John] 

Name field in PostForm:[John] 

Hobbies field in FormValue:sports
```

+   传递 x-www-form-urlencoded 请求体。在查询字符串中传递一个额外字段 **gender**。在输出中注意，性别字段不在 **PostForm** 中，但在 **Form** 中。

```go
curl -v -X POST 'http://localhost:8080/employee?gender=male' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'name=John' \
--data-urlencode 'age=18' \
--data-urlencode 'hobbies=sports' \
--data-urlencode 'hobbies=music'
```

**输出**

```go
request.Form:: 
Key:age, Value:[18] 
Key:hobbies, Value:[sports music] 
Key:gender, Value:[male] 
Key:name, Value:[John] 

request.PostForm:: 
Key:name, Value:[John] 
Key:age, Value:[18] 
Key:hobbies, Value:[sports music] 

Name field in Form:[John] 

Name field in PostForm:[John] 

Hobbies field in FormValue:sports
```

+   传递 x-www-form-urlencoded 请求体。同时在查询字符串中传递相同的键，即 **age**。在查询字符串中传递 **age=20**，在请求体中传递 **age=18**。**age** 的值 **20** 将不在 **PostForm** 字段中，但会在 **Form** 字段中。由于请求体参数优先，因此 **age=18** 是 **Form** 字段数组中的第一个值。如果你运行 **r.FormValue(“age”)**，则将返回 18 而不是 20。

```go
curl -v -X POST 'http://localhost:8080/employee?age=20' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'name=John' \
--data-urlencode 'age=18' \
--data-urlencode 'hobbies=sports' \
--data-urlencode 'hobbies=music'
```

**输出**

```go
request.Form:: 
Key:hobbies, Value:[sports music] 
Key:name, Value:[John] 
Key:age, Value:[18 20] 

request.PostForm:: 
Key:name, Value:[John] 
Key:age, Value:[18] 
Key:hobbies, Value:[sports music] 

Name field in Form:[John] 

Name field in PostForm:[John] 

Hobbies field in FormValue:sports
```


