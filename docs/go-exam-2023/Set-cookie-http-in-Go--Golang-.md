<!--yml

类别：未分类

日期：2024-10-13 06:35:47

-->

# 在Go语言中设置http cookie

> 来源：[https://golangbyexample.com/set-cookie-http-golang/](https://golangbyexample.com/set-cookie-http-golang/)

目录

**   [概述](#Overview "Overview")

+   [为某个请求设置cookie](#Set_cookie_for_one_of_the_request "Set cookie for one of the request")

+   [为该域的所有请求设置cookie并发送该cookie](#Set_cookie_and_send_that_cookie_for_all_requests_to_that_domain "Set cookie and send that cookie for all requests to that domain")

    +   [Client](#Client "Client")

    +   [Server](#Server "Server")*  *## **概述**

在golang中，cookie的表示如下：

[https://golang.org/src/net/http/cookie.go](https://golang.org/src/net/http/cookie.go)

```
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

详见[https://tools.ietf.org/html/rfc6265](https://tools.ietf.org/html/rfc6265)了解上述cookie的每个字段的详细信息。

在设置cookies时，有两种情况。

+   你希望只为特定域的一个请求设置cookie。

+   你希望设置cookies，并希望该cookie在所有请求中发送到该域。

第二个用例是在第一次调用中通过输入用户名和密码生成身份验证令牌时需要的，你希望该令牌在每个后续调用中通过cookie传递。我们将为此用例使用cookie jar。

让我们逐一查看这两种用例。

## **为某个请求设置cookie**

这是golang作为HTTP客户端的情况。可以使用**net/http**包的**AddCookie**方法添加cookie。如果我们为两个不同的名称和值调用此方法，则这两个名称和值将被添加到结果cookie中。

```
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

```
token=some_token
clicked=true
StatusCode: 200
```

我们首先需要创建cookie的实例，如下所示。

[https://golang.org/src/net/http/cookie.go](https://golang.org/src/net/http/cookie.go)

```
cookie := &http.Cookie{
    Name:   "token",
    Value:  "some_token",
    MaxAge: 300,
}
```

一旦创建了cookie，就使用**AddCookie**方法将其添加到HTTP请求对象中。

```
req.AddCookie(cookie)
```

在上述程序中，HTTP客户端添加了两个带有以下名称-值对的cookie。

+   token=some_token

+   clicked=true

这两个cookie将在对[google.com](http://google.com)的调用中发送。

## ****为该域的所有请求设置cookie并发送该cookie****

golang中的HTTP客户端允许你指定一个**CookieJar**，可用于在进行外部HTTP请求时存储和发送cookies。顾名思义，可以把它想象成一个装有cookies的罐子。

[https://golang.org/pkg/net/http/#Client](https://golang.org/pkg/net/http/#Client)

以下是net/http **Client**结构的结构。它包含一个名为**Jar**的实例变量，类型为**CookieJar**，这是一个接口。

```
type Client struct {
    Transport RoundTripper

    CheckRedirect func(req *Request, via []*Request) error

    Jar CookieJar

    Timeout time.Duration
}
```

以下是**CookieJar**接口

```
type CookieJar interface {
    SetCookies(u *url.URL, cookies []*Cookie)
    Cookies(u *url.URL) []*Cookie
}
```

net/http提供了一个默认的cookie jar实现，符合上述**CookieJar**接口。我们将在初始化我们的net/http客户端时使用它。

[https://golang.org/pkg/net/http/cookiejar/#Jar](https://golang.org/pkg/net/http/cookiejar/#Jar)

你还可以在初始化net/http Client结构时提供自定义cookie jar，该结构实现了上述**CookieJar**接口。

HTTP客户端以两种方式使用这个jar。

+   正在向这个Jar添加cookies。你可以明确地将cookies添加到这个jar中。如果服务器在响应头中发送Set-Cookies头，cookies也会被添加到jar中。在**Set-Cookie**头中指定的所有cookies都会被添加。有关Set-Cookie头的更多细节，请参阅这个[链接](https://golangbyexample.com/set-cookie-response-header/)。这个链接包含了理解golang中**Set-Cookie**头的所有细节。

+   在进行任何外部HTTP请求时，咨询这个jar。它会检查这个jar，以了解需要为特定域发送哪些cookies。

让我们看看一个示例，客户端在进行HTTP请求时将添加一个cookie。这个cookie将会在所有后续请求中发送到同一域。

在这个示例中，我们将看到客户端如何在cookie jar中设置cookie。为此，让我们首先创建一个客户端。以下是相应的程序。

这是客户端代码。

### **客户端**

**go.mod**

```
module sample.com/client
go 1.16
```

**client.go**

```
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

在上述客户端程序中，我们正在创建一个带有cookie Jar的HTTP客户端。

```
jar, err := cookiejar.New(nil)
if err != nil {
     log.Fatalf("Got error while creating cookie jar %s", err.Error())
}

client = http.Client{
     Jar: jar,
}
```

我们正在向Cookie Jar添加一个cookie。

```
cookie := &http.Cookie{
		Name:   "token",
		Value:  "some_token",
		MaxAge: 300,
}
urlObj, _ := url.Parse("http://localhost:8080/")
client.Jar.SetCookies(urlObj, []*http.Cookie{cookie})
```

为了测试上述代码，并说明在第一次调用中添加到cookie jar中的cookies确实会在后续调用中发送，我们还需要创建服务器，以便打印传入的cookies。

### **服务器**

服务器监听8080端口并有两个API。这是客户端调用的两个API。

+   **localhost:8080/doc**

+   **localhost:8080/doc/id**

在这两个API中，我们打印接收到的传入头中的cookies。

**go.mod**

```
module sample.com/server

go 1.16
```

**server.go**

```
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

```
go run server.go
```

以及客户端。

```
go run client.go
```

注意服务器端的输出。

```
Cookie in First API Call
token=some_token

Cookie in Second API Call
token=some_token
```

同样的cookie会在客户端对服务器的第一次和第二次调用中自动发送。这是如何开箱即用的？这是因为**CookieJar**的介入。golang HTTP客户端在进行HTTP调用之前会检查Cookie Jar，然后发送这个cookie。

这就是在golang中设置cookies的全部内容。希望你喜欢这个教程。请在评论中分享反馈。

此外，请查看我们的Golang高级教程系列 – [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [cookie](https://golangbyexample.com/tag/cookie/) *   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/) *   [set](https://golangbyexample.com/tag/set/) *
