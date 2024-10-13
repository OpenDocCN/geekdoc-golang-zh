<!--yml

分类：未分类

日期：2024-10-13 06:35:28

-->

# Go中的CookieJar（Golang）

> 来源：[https://golangbyexample.com/cookiejar-golang/](https://golangbyexample.com/cookiejar-golang/)

目录

**   [概述](#Overview "Overview")

+   [第一个示例](#First_example "First example")

    +   [服务器](#Server "Server")

    +   [客户端](#Client "Client")

+   [第二个示例](#Second_Example "Second Example")

    +   [服务器](#Server-2 "Server")

    +   [客户端](#Client-2 "Client")*  *## **概述**

Golang中的HTTP客户端允许您指定一个**CookieJar**，用于管理在进行外部HTTP请求时的cookies存储和发送。如其名所示，可以将其视为一个包含cookies的罐子。

[https://golang.org/pkg/net/http/#Client](https://golang.org/pkg/net/http/#Client)

以下是net/http **Client**结构的结构。它包含一个名为**Jar**的实例变量，其类型为**CookieJar**接口。

```go
type Client struct {
    Transport RoundTripper

    CheckRedirect func(req *Request, via []*Request) error

    Jar CookieJar

    Timeout time.Duration
}
```

以下是**CookieJar**接口。

```go
type CookieJar interface {
    SetCookies(u *url.URL, cookies []*Cookie)
    Cookies(u *url.URL) []*Cookie
}
```

net/http提供了一个默认的cookie jar实现，符合上述**CookieJar**接口。我们在初始化net/http客户端时将使用它。

[https://golang.org/pkg/net/http/cookiejar/#Jar](https://golang.org/pkg/net/http/cookiejar/#Jar)

您还可以在初始化net/http Client结构时提供自定义cookie jar，该结构实现了上述**CookieJar**接口。

HTTP客户端以两种方式使用此jar。

+   向此Jar中添加cookies。您可以显式地将cookies添加到此jar中。如果服务器在响应头中发送Set-Cookies头，则这些cookies也将被添加到jar中。Set-Cookie头中指定的所有cookies都将被添加。

+   在进行任何外部HTTP请求时，查询此jar。它检查此jar以了解需要为特定域发送哪些cookies。

让我们通过几个示例来说明cookie jar。

在第一个示例中，客户端将在进行HTTP请求时添加一个cookie。此cookie将在后续所有请求中发送到同一域。

在第二个示例中，我们将看到服务器发送**Set-Cookie**头，该cookie将在客户端设置。

## **第一个示例**

在本示例中，我们将看到客户端如何在cookie jar中设置cookie。首先，让我们创建一个服务器，以下是相关程序。

### **服务器**

服务器监听8080端口，并有两个API。

+   **localhost:8080/doc**

+   **localhost:8080/doc/id**

在这两个API中，我们打印接收到的请求头中的cookies。

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

在上述客户端程序中，我们创建了一个带有cookie jar的HTTP客户端。

```go
jar, err := cookiejar.New(nil)
if err != nil {
		log.Fatalf("Got error while creating cookie jar %s", err.Error())
}

client = http.Client{
		Jar: jar,
}
```

我们正在向Cookie Jar添加一个cookie。

```go
cookie := &http.Cookie{
		Name:   "token",
		Value:  "some_token",
		MaxAge: 300,
}
urlObj, _ := url.Parse("http://localhost:8080/")
client.Jar.SetCookies(urlObj, []*http.Cookie{cookie})
```

现在运行服务器。

```go
go run server.go
```

和客户端。

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

相同的 cookie 在客户端对服务器的第一次和第二次调用中自动发送。它是如何开箱即用的？这是因为**CookieJar**参与了这个过程。golang HTTP 客户端在进行 HTTP 调用之前检查 Cookie Jar。然后发送这个 cookie。

## **第二个示例**

在第二个示例中，我们将看到服务器在 Set-Cookie 头中发送的 cookie 如何被保存到**CookieJar**中。然后它将在后续调用中发送。为了说明这一点，让我们也创建一个将发送 Set-Cookie 头的服务器。以下是服务器代码。

### **服务器**

我们将创建一个服务器，创建两个 API。

+   **localhost:8080/doc** – 在这个 API 中，服务器将在响应中设置**Set-Cookie**头。我们将从 golang 程序发起这个调用。golang http 客户端将在其端保存这个 cookie。然后客户端将为对 localhost:8080 的任何其他请求发送相同的 cookie 回到服务器。

+   **localhost:8080/doc/id** – 这是一个示例 API，用于演示 golang http 客户端如何实际使用**CookieJar**在请求中发送在 Set-Cookie 头中接收到的相同 cookie。注意在第二个 API 的代码中，我们接收到的所有 cookies。

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
	cookie := &http.Cookie{
		Name:   "id",
		Value:  "abcd",
		MaxAge: 300,
	}
	http.SetCookie(w, cookie)
	w.WriteHeader(200)
	w.Write([]byte("Doc Get Successful"))
	return
}

func docGetID(w http.ResponseWriter, r *http.Request) {
	for _, c := range r.Cookies() {
		fmt.Println(c)
	}
	w.WriteHeader(200)
	w.Write([]byte("Doc Get ID Successful"))
	return
}
```

我们在响应头的**Set-Cookie**中设置了以下 cookie。

```go
cookie := &http.Cookie{
	Name:   "id",
	Value:  "abcd",
	MaxAge: 300,
}
http.SetCookie(w, cookie)
```

这是客户端代码。

### 客户端

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

在客户端，我们只是创建了一个指定了 CookieJar 的 HTTP 客户端。除此之外，我们只是进行了两个 API 调用。

+   首先 [http://localhost:8080/doc](http://localhost:8080/doc)

+   第二个 [http://localhost:8080/doc/id](http://localhost:8080/doc/id)

现在首先运行服务器。它将在 8080 端口启动一个本地服务器。

```go
go run server.go
```

现在运行客户端

```go
go run client.go
```

注意服务器端第二个 API 的输出。可以看到，它是响应头中**Set-Cookie**返回的相同 cookie。

```go
Printing the cookies in the Second API
id=abcd
```

这是它开箱即用的工作方式。

+   在第一次 API 调用中，golang http 客户端将**Set-Cookie**响应头中存在的所有 cookies 保存到**CookieJar**中。

+   在进行第二次 API 调用之前，它会检查**CookieJar**以确定需要发送给服务器的所有 cookies。然后，它会发送这些 cookies。

这就是关于 golang 中 cookie jar 的全部内容。希望你喜欢这个教程。另请查看我们的 Golang 高级教程系列 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
