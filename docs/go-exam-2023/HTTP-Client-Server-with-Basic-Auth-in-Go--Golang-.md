<!--yml

类别：未分类

日期：2024-10-13 06:32:18

-->

# Go（Golang）中的带基本身份验证的 HTTP 客户端/服务器。

> 来源：[`golangbyexample.com/http-basic-auth-golang/`](https://golangbyexample.com/http-basic-auth-golang/)

目录

**概述**

+   HTTP 服务器基本身份验证示例

+   HTTP 客户端基本身份验证示例 * * # **概述**

基本身份验证是为网络服务器上的资源提供访问控制的最简单形式。基本访问认证是在进行 HTTP 请求时向服务器提供用户名和密码的一种方式。凭据在请求的头部发送。以下是发送凭据的头部和格式。

```go
Authorization : Basic <credentials></credentials>
```

+   头部名称是 **“Authorization”**

+   凭据是通过冒号（:）连接的用户名和密码进行 Base64 编码发送的。基本上是对以下字符串的 Base64 编码。

```go
<username>:<password></password></username>
```

+   凭据以 **Basic** 开头。

基本身份验证在 RFC 7617 中有说明。

[`tools.ietf.org/html/rfc7617`](https://tools.ietf.org/html/rfc7617)

基本身份验证不需要任何类型的会话标识符或 Cookies。此外，由于凭据仅作为 Base64 编码发送，因此没有加密涉及。因此，基本身份验证仅在 HTTPS 中使用以确保安全。

# **HTTP 服务器基本身份验证示例**

首先，让我们看看与 **HTTP** 服务器相关的基本身份验证。Golang 的 **net/http** 包提供了一种在 ***http.Request** 结构上定义的方法，可以返回传入请求的 **Authorization** 头中存在的用户名和密码。以下是该方法的签名。

```go
func (r *Request) BasicAuth() (username, password string, ok bool)
```

这个方法的作用是检查 **Authorization** 头，然后从 Base64 编码值中提取用户名和密码并返回。如果解析时出现任何问题，它将返回 **ok** 变量为 false。因此，在使用此函数时，我们首先需要检查 **ok** 变量的值。如果 **ok** 变量为 true，则可以进一步匹配用户名和密码并验证其是否正确。

让我们看看一个程序。

```go
package main

import (
	"fmt"
	"net/http"
)

var (
	username = "abc"
	password = "123"
)

func main() {
	handler := http.HandlerFunc(handleRequest)
	http.Handle("/example", handler)
	http.ListenAndServe(":8080", nil)
}

func handleRequest(w http.ResponseWriter, r *http.Request) {

	u, p, ok := r.BasicAuth()
	if !ok {
		fmt.Println("Error parsing basic auth")
		w.WriteHeader(401)
		return
	}
	if u != username {
		fmt.Printf("Username provided is correct: %s\n", u)
		w.WriteHeader(401)
		return
	}
	if p != password {
		fmt.Printf("Password provided is correct: %s\n", u)
		w.WriteHeader(401)
		return
	}
	fmt.Printf("Username: %s\n", u)
	fmt.Printf("Password: %s\n", p)
	w.WriteHeader(200)
	return
}
```

服务器接受以下用户名和密码。

+   用户名是 abc。

+   密码是 123。

为了发出请求，我们必须对以下字符串进行 Base64 编码，该字符串是由一个冒号（:）连接的用户名和密码。

```go
abc:123
```

上述字符串的 Base64 编码将是。

```go
YWJjOjEyMw==
```

你可以在这里查看 – [`www.base64encode.org/`](https://www.base64encode.org/)

现在发出以下请求。

```go
curl -v -X POST http://localhost:8080/example -H "Authorization: Basic YWJjOjEyMw=="
```

此请求将获得状态代码 200。同时，它还会在日志中正确打印用户名和密码。

```go
Username: abc
Password: 123
```

现在发送格式错误的 **Base64** 编码值。

```go
curl -v -X POST http://localhost:8080/example -H "Authorization: Basic YWJjOjEy"
```

它将获得状态代码 401。它还将在日志中打印以下内容。

```go
Error parsing basic auth
```

# **HTTP 客户端基本身份验证示例**

Golang 的 **net/http** 包还提供了一种在 ***http.Request** 结构上定义的方法，可用于设置基本身份验证头。以下是该方法的签名。

```go
func (r *Request) SetBasicAuth(username, password string)
```

该方法的作用是接收**用户名**和**密码**，并使用以冒号（:）连接的用户名和密码的 base64 编码值设置 Authorization 头。

让我们看看一个程序。

```go
package main

import (
	"fmt"
	"net/http"
	"time"
)

var (
	username = "abc"
	password = "123"
)

func main() {
	call("https://localhost:8080/example", "POST")
}

func call(url, method string) error {
	client := &http.Client{
		Timeout: time.Second * 10,
	}
	req, err := http.NewRequest(method, url, nil)
	if err != nil {
		return fmt.Errorf("Got error %s", err.Error())
	}
	req.SetBasicAuth(username, password)
	response, err := client.Do(req)
	if err != nil {
		return fmt.Errorf("Got error %s", err.Error())
	}
	defer response.Body.Close()
	return nil
}
```

在上面的程序中，我们正在调用之前设置的服务器。看看我们如何在发出的请求中设置基本认证。

```go
req.SetBasicAuth(username, password)
```

如果你运行上面的程序，你将从我们之前设置的服务器收到 200 的响应。

**注意：** 我们在上面打印**用户名**和**密码**仅用于示例目的。在实际程序中，它们需要以某种安全的方式加密存储。同时，我们在下面发送的是 http 请求而非 https 用于基本认证。这仅用于说明，客户端和服务器位于同一台机器上。这并不推荐，基本认证应仅在**HTTPS**下使用。

+   [去](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
