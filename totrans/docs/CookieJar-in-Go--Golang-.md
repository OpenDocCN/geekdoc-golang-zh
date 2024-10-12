<!--yml
category: 未分类
date: 2024-10-13 06:35:28
-->

# CookieJar in Go (Golang)

> 来源：[https://golangbyexample.com/cookiejar-golang/](https://golangbyexample.com/cookiejar-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [First example](#First_example "First example")
    *   [Server](#Server "Server")
    *   [Client](#Client "Client")
*   [Second Example](#Second_Example "Second Example")
    *   [Server](#Server-2 "Server")
    *   [Client](#Client-2 "Client")*  *## **Overview**

HTTP client in golang lets you specify a **CookieJar** that manages storing and sending of the cookies while making external HTTP requests. As the name suggests, think of it as a jar that contains cookies.

[https://golang.org/pkg/net/http/#Client](https://golang.org/pkg/net/http/#Client)

Below is the structure of the net/http **Client** struct. It contains an instance variable named **Jar** of type **CookieJar** which is an interface

```
type Client struct {
    Transport RoundTripper

    CheckRedirect func(req *Request, via []*Request) error

    Jar CookieJar

    Timeout time.Duration
}
```

Below is the **CookieJar** interface

```
type CookieJar interface {
    SetCookies(u *url.URL, cookies []*Cookie)
    Cookies(u *url.URL) []*Cookie
}
```

net/http provides a default cookie jar implementation which implements the above **CookieJar** interface. We will use it while initializing our net/http Client

[https://golang.org/pkg/net/http/cookiejar/#Jar](https://golang.org/pkg/net/http/cookiejar/#Jar)

You can also provide your custom cookie jar during the initialization of net/http Client struct which implements the above **CookieJar** interface.

HTTP client uses this jar in two ways

*   Adding cookies in this Jar. You can explicitly add cookies to this jar. The cookies will also be added to the jar if the server sends the Set-Cookies header in the response headers. All the cookies specified in the Set-Cookie header will be added 

*   For consulting this jar while making any external HTTP requests. It checks this jar to know what all cookies it needs to send for a particular domain

Let’s illustrate the cookie jar with a couple of examples. 

In the first example, the client will add a cookie while making HTTP requests. This cookie will be sent in all subsequent requests to the same domain.

In the second example, we will see that the server sends the **Set-Cookie** header and that cookie will be set at the client end.

## **First example**

In this example, we will see how the client can set the cookie in the cookie jar. For that let’s first create a server Here is the program for the same

### **Server**

The server listens to port 8080 and has two APIs

*   **localhost:8080/doc**

*   **localhost:8080/doc/id**

In both the APIs we are printing the cookies which it receives in the incoming header

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

Here is the client code

### **Client**

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

In the above client program, we are creating an HTTP client with a cookie Jar

```
jar, err := cookiejar.New(nil)
if err != nil {
		log.Fatalf("Got error while creating cookie jar %s", err.Error())
}

client = http.Client{
		Jar: jar,
}
```

we are adding a cookie to the Cookie Jar

```
cookie := &http.Cookie{
		Name:   "token",
		Value:  "some_token",
		MaxAge: 300,
}
urlObj, _ := url.Parse("http://localhost:8080/")
client.Jar.SetCookies(urlObj, []*http.Cookie{cookie})
```

Now run the server

```
go run server.go
```

and client

```
go run client.go
```

Notice the output at the server end

```
Cookie in First API Call
token=some_token

Cookie in Second API Call
token=some_token
```

The same cookie is being sent automatically in both the first and second call which the client will make to the server. How does it work out of the box? This is because **CookieJar** comes into the picture. The golang HTTP client checks the Cookie Jar before making the HTTP call. It then sends this cookie.

## **Second Example**

In the second example, we will see how cookie sent by the server in the Set-Cookie header is saved in the **CookieJar**. And then it will be sent in the subsequent calls.  To illustrate that let’s also create a server that will send the Set-Cookie header. Here is the server code

### **Server**

We will create a server Create two APIs

*   **localhost:8080/doc** – In this API the server will set the **Set-Cookie** header in the response. We are going to make this call from the golang program. The golang http client is going to save this cookie at its end. The client will then send the same cookie back to the server for any other request to localhost:8080

*   **localhost:8080/doc/id** – This is the example API to demonstrate that golang http client will actually use the **CookieJar** to send the same cookie in the request that is received in response in the Set-Cookie header. Notice in the code for the second API that we are all the cookies that it receives

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

We are setting the below cookie in **Set-Cookie** in response headers

```
cookie := &http.Cookie{
	Name:   "id",
	Value:  "abcd",
	MaxAge: 300,
}
http.SetCookie(w, cookie)
```

Here is the client code

### Client

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

At the client end, we just created an HTTP client with a CookieJar specified. Other than that we are just making two API calls.

*   First [http://localhost:8080/doc](http://localhost:8080/doc)
*   Second [http://localhost:8080/doc/id](http://localhost:8080/doc/id)

Now run server first. It will start a local server listening on port 8080

```
go run server.go
```

Now run the client

```
go run client.go
```

Notice the output at the server end for the second API. See that it is the same cookie that was returned in the **Set-Cookie** response headers.

```
Printing the cookies in the Second API
id=abcd
```

This is how it works out of the box

*   In the first API call, the golang http client saves all the cookie present in **Set-Cookie** response header in the **CookieJar**
*   Before making the second API call it checks the **CookieJar** to figure out what all cookies need to be sent to the server. It then sends those cookies.

This is all about cookie jar in golang. Hope you have liked this tutorial. Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*