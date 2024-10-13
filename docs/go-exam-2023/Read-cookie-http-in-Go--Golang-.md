<!--yml

类别：未分类

日期：2024-10-13 06:35:41

-->

# 在 Go（Golang）中读取 cookie http

> 来源：[`golangbyexample.com/read-cookie-http-golang/`](https://golangbyexample.com/read-cookie-http-golang/)

目录

+   概述

+   程序

## **概述**

net/http Request 结构体提供了一种方便的方法，可以根据名称读取特定 cookie。以下是该方法的签名。[`golang.org/pkg/net/http/#Request.Cookie`](https://golang.org/pkg/net/http/#Request.Cookie)

```go
func (r *Request) Cookie(name string) (*Cookie, error)
```

要打印所有 cookies，我们可以遍历**http.Request**结构体的**Cookies**方法。我们可以为此使用 range 关键字。

```go
for _, c := range r.Cookies() {
     fmt.Println(c)
}
```

这两个函数将返回**Cookie**结构体的实例。在 golang 中，cookie 表示如下。

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

有关上述 cookie 中每个字段的详细信息，请参见[`tools.ietf.org/html/rfc6265`](https://tools.ietf.org/html/rfc6265)。

## **程序**

以下是相同程序，展示**Cookie**和**Cookies**方法的**http.Request**结构体

```go
package main

import (
	"fmt"
	"log"
	"net/http"
)

func main() {
	docHandler := http.HandlerFunc(docHandler)
	http.Handle("/doc", docHandler)

	http.ListenAndServe(":8080", nil)
}

func docHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Println("Cookies in API Call:")

	tokenCookie, err := r.Cookie("token")
	if err != nil {
		log.Fatalf("Error occured while reading cookie")
	}
	fmt.Println("\nPrinting cookie with name as token")
	fmt.Println(tokenCookie)

	fmt.Println("\nPrinting all cookies")
	for _, c := range r.Cookies() {
		fmt.Println(c)
	}
	fmt.Println()
	w.WriteHeader(200)
	w.Write([]byte("Doc Get Successful"))
	return
}
```

运行上面的程序并进行以下 curl 调用

```go
curl -X GET localhost:8080/doc --cookie "id=abcd; token=some_token"
```

curl 调用传递了两个 cookie 名称-值对

+   id=abcd

+   token=some_token

它将给出以下输出

```go
Cookies in API Call:

Printing cookie with name as "token"
token=some_token

Printing all cookies
id=abcd
token=some_token
```

这就是我们如何打印名为**“token”**的特定 cookie

```go
tokenCookie, err := r.Cookie("token")
```

它的输出如所见

```go
token=some_token
```

这就是我们如何打印所有的 cookie

```go
for _, c := range r.Cookies() {
     fmt.Println(c)
}
```

它输出我们在 curl 调用中发送的 cookie 名称-值对

```go
id=abcd
token=some_token
```

这就是关于 golang 中 cookies 的所有内容。希望你喜欢这个教程。请在评论中分享反馈。

此外，请查看我们的 Golang 高级教程系列 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [cookie](https://golangbyexample.com/tag/cookie/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
