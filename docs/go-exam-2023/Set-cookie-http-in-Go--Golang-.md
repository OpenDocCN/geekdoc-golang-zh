<!--yml

类别：未分类

日期：2024-10-13 06:35:47

-->

# 在 Go 语言中设置 http cookie

> 来源：[`golangbyexample.com/set-cookie-http-golang/`](https://golangbyexample.com/set-cookie-http-golang/)

目录

**   概述

+   为某个请求设置 cookie

+   为该域的所有请求设置 cookie 并发送该 cookie

    +   Client

    +   Server*  *## **概述**

在 golang 中，cookie 的表示如下：

[`golang.org/src/net/http/cookie.go`](https://golang.org/src/net/http/cookie.go)

```go
type Cookie struct {
	Name  string
	Value string

	Path       string    // optional
	Domain     string    // optional
	Expires    time.Time // optional
	RawExpires string    // for reading cookies only

	// MaxAge=0 means no 'Max-Age' attribute specified.
	// MaxAge<0 means delete cookie now, equivalently 'Max-Age: 0'
	// MaxAge>0 means Max-Age attribute present and given in seconds
	MaxAge   int
	Secure   bool
	HttpOnly bool
	SameSite SameSite
	Raw      string
	Unparsed []string // Raw text of unparsed attribute-value pairs
}
```

详见[`tools.ietf.org/html/rfc6265`](https://tools.ietf.org/html/rfc6265)了解上述 cookie 的每个字段的详细信息。

在设置 cookies 时，有两种情况。

+   你希望只为特定域的一个请求设置 cookie。

+   你希望设置 cookies，并希望该 cookie 在所有请求中发送到该域。

第二个用例是在第一次调用中通过输入用户名和密码生成身份验证令牌时需要的，你希望该令牌在每个后续调用中通过 cookie 传递。我们将为此用例使用 cookie jar。

让我们逐一查看这两种用例。

## **为某个请求设置 cookie**

这是 golang 作为 HTTP 客户端的情况。可以使用**net/http**包的**AddCookie**方法添加 cookie。如果我们为两个不同的名称和值调用此方法，则这两个名称和值将被添加到结果 cookie 中。

```go
package main
import (
    "fmt"
    "log"
    "net/http"
    "net/http/cookiejar"
)
var client http.Client
func init() {
    jar, err := cookiejar.New(nil)
    if err != nil {
        log.Fatalf("Got error while creating cookie jar %s", err.Error())
    }
    client = http.Client{
        Jar: jar,
    }
}
func main() {
    cookie := &http.Cookie{
        Name:   "token",
        Value:  "some_token",
        MaxAge: 300,
    }
    cookie2 := &http.Cookie{
        Name:   "clicked",
        Value:  "true",
        MaxAge: 300,
    }
    req, err := http.NewRequest("GET", "http://google.com", nil)
    if err != nil {
        log.Fatalf("Got error %s", err.Error())
    }
    req.AddCookie(cookie)
    req.AddCookie(cookie2)
    for _, c := range req.Cookies() {
        fmt.Println(c)
    }
    resp, err := client.Do(req)
    if err != nil {
        log.Fatalf("Error occured. Error is: %s", err.Error())
    }
    defer resp.Body.Close()
    fmt.Printf("StatusCode: %d\n", resp.StatusCode)
}
```

**输出**

```go
token=some_token
clicked=true
StatusCode: 200
```

我们首先需要创建 cookie 的实例，如下所示。

[`golang.org/src/net/http/cookie.go`](https://golang.org/src/net/http/cookie.go)

```go
cookie := &http.Cookie{
    Name:   "token",
    Value:  "some_token",
    MaxAge: 300,
}
```

一旦创建了 cookie，就使用**AddCookie**方法将其添加到 HTTP 请求对象中。

```go
req.AddCookie(cookie)
```

在上述程序中，HTTP 客户端添加了两个带有以下名称-值对的 cookie。

+   token=some_token

+   clicked=true

这两个 cookie 将在对[google.com](http://google.com)的调用中发送。

## ****为该域的所有请求设置 cookie 并发送该 cookie****

golang 中的 HTTP 客户端允许你指定一个**CookieJar**，可用于在进行外部 HTTP 请求时存储和发送 cookies。顾名思义，可以把它想象成一个装有 cookies 的罐子。

[`golang.org/pkg/net/http/#Client`](https://golang.org/pkg/net/http/#Client)

以下是 net/http **Client**结构的结构。它包含一个名为**Jar**的实例变量，类型为**CookieJar**，这是一个接口。

```go
type Client struct {
    Transport RoundTripper

    CheckRedirect func(req *Request, via []*Request) error

    Jar CookieJar

    Timeout time.Duration
}
```

以下是**CookieJar**接口

```go
type CookieJar interface {
    SetCookies(u *url.URL, cookies []*Cookie)
    Cookies(u *url.URL) []*Cookie
}
```

net/http 提供了一个默认的 cookie jar 实现，符合上述**CookieJar**接口。我们将在初始化我们的 net/http 客户端时使用它。

[`golang.org/pkg/net/http/cookiejar/#Jar`](https://golang.org/pkg/net/http/cookiejar/#Jar)

你还可以在初始化 net/http Client 结构时提供自定义 cookie jar，该结构实现了上述**CookieJar**接口。

HTTP 客户端以两种方式使用这个 jar。

+   正在向这个 Jar 添加 cookies。你可以明确地将 cookies 添加到这个 jar 中。如果服务器在响应头中发送 Set-Cookies 头，cookies 也会被添加到 jar 中。在**Set-Cookie**头中指定的所有 cookies 都会被添加。有关 Set-Cookie 头的更多细节，请参阅这个[链接](https://golangbyexample.com/set-cookie-response-header/)。这个链接包含了理解 golang 中**Set-Cookie**头的所有细节。

+   在进行任何外部 HTTP 请求时，咨询这个 jar。它会检查这个 jar，以了解需要为特定域发送哪些 cookies。

让我们看看一个示例，客户端在进行 HTTP 请求时将添加一个 cookie。这个 cookie 将会在所有后续请求中发送到同一域。

在这个示例中，我们将看到客户端如何在 cookie jar 中设置 cookie。为此，让我们首先创建一个客户端。以下是相应的程序。

这是客户端代码。

### **客户端**

**go.mod**

```go
module sample.com/client
go 1.16
```

**client.go**

```go
package main

import (
	"fmt"
	"log"
	"net/http"
	"net/http/cookiejar"
	"net/url"
)

var client http.Client

func init() {
	jar, err := cookiejar.New(nil)
	if err != nil {
		log.Fatalf("Got error while creating cookie jar %s", err.Error())
	}

	client = http.Client{
		Jar: jar,
	}
}

func main() {
	req, err := http.NewRequest("GET", "http://localhost:8080/doc", nil)
	if err != nil {
		log.Fatalf("Got error %s", err.Error())
	}
	cookie := &http.Cookie{
		Name:   "token",
		Value:  "some_token",
		MaxAge: 300,
	}
	urlObj, _ := url.Parse("http://localhost:8080/")
	client.Jar.SetCookies(urlObj, []*http.Cookie{cookie})
	resp, err := client.Do(req)
	if err != nil {
		log.Fatalf("Error occured. Error is: %s", err.Error())
	}
	defer resp.Body.Close()

	fmt.Printf("StatusCode: %d\n", resp.StatusCode)

	req, err = http.NewRequest("GET", "http://localhost:8080/doc/id", nil)
	if err != nil {
		log.Fatalf("Got error %s", err.Error())
	}

	resp, err = client.Do(req)
	if err != nil {
		log.Fatalf("Error occured. Error is: %s", err.Error())
	}
	defer resp.Body.Close()

	fmt.Printf("StatusCode: %d\n", resp.StatusCode)
}
```

在上述客户端程序中，我们正在创建一个带有 cookie Jar 的 HTTP 客户端。

```go
jar, err := cookiejar.New(nil)
if err != nil {
     log.Fatalf("Got error while creating cookie jar %s", err.Error())
}

client = http.Client{
     Jar: jar,
}
```

我们正在向 Cookie Jar 添加一个 cookie。

```go
cookie := &http.Cookie{
		Name:   "token",
		Value:  "some_token",
		MaxAge: 300,
}
urlObj, _ := url.Parse("http://localhost:8080/")
client.Jar.SetCookies(urlObj, []*http.Cookie{cookie})
```

为了测试上述代码，并说明在第一次调用中添加到 cookie jar 中的 cookies 确实会在后续调用中发送，我们还需要创建服务器，以便打印传入的 cookies。

### **服务器**

服务器监听 8080 端口并有两个 API。这是客户端调用的两个 API。

+   **localhost:8080/doc**

+   **localhost:8080/doc/id**

在这两个 API 中，我们打印接收到的传入头中的 cookies。

**go.mod**

```go
module sample.com/server

go 1.16
```

**server.go**

```go
package main

import (
	"fmt"
	"net/http"
)

func main() {
	docHandler := http.HandlerFunc(docHandler)
	http.Handle("/doc", docHandler)

	docGetID := http.HandlerFunc(docGetID)
	http.Handle("/doc/id", docGetID)

	http.ListenAndServe(":8080", nil)
}

func docHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Println("Cookie in First API Call")
	for _, c := range r.Cookies() {
		fmt.Println(c)
	}
	fmt.Println()
	w.WriteHeader(200)
	w.Write([]byte("Doc Get Successful"))
	return
}

func docGetID(w http.ResponseWriter, r *http.Request) {
	fmt.Println("Cookie in Second API Call")
	for _, c := range r.Cookies() {
		fmt.Println(c)
	}
	w.WriteHeader(200)
	w.Write([]byte("Doc Get ID Successful"))
	return
}
```

现在运行服务器。

```go
go run server.go
```

以及客户端。

```go
go run client.go
```

注意服务器端的输出。

```go
Cookie in First API Call
token=some_token

Cookie in Second API Call
token=some_token
```

同样的 cookie 会在客户端对服务器的第一次和第二次调用中自动发送。这是如何开箱即用的？这是因为**CookieJar**的介入。golang HTTP 客户端在进行 HTTP 调用之前会检查 Cookie Jar，然后发送这个 cookie。

这就是在 golang 中设置 cookies 的全部内容。希望你喜欢这个教程。请在评论中分享反馈。

此外，请查看我们的 Golang 高级教程系列 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [cookie](https://golangbyexample.com/tag/cookie/) *   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/) *   [set](https://golangbyexample.com/tag/set/) *
