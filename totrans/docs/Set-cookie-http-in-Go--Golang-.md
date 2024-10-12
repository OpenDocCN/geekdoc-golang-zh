<!--yml
category: 未分类
date: 2024-10-13 06:35:47
-->

# Set cookie http in Go (Golang)

> 来源：[https://golangbyexample.com/set-cookie-http-golang/](https://golangbyexample.com/set-cookie-http-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Set cookie for one of the request](#Set_cookie_for_one_of_the_request "Set cookie for one of the request")
*   [Set cookie and send that cookie for all requests to that domain](#Set_cookie_and_send_that_cookie_for_all_requests_to_that_domain "Set cookie and send that cookie for all requests to that domain")
    *   [Client](#Client "Client")
    *   [Server](#Server "Server")*  *## **Overview**

A cookie in golang is represented as below in golang

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

See [https://tools.ietf.org/html/rfc6265](https://tools.ietf.org/html/rfc6265) for details of each of the fields of the above cookie.

When it comes to the setting of cookies, there are two cases

*   You want to set cookie only for one of the requests for a particular domain

*   You want to set cookies and you want that cookie to be sent for all requests to that domain.

The second use case is needed when let’s say you generate an authentication token in the first call by entering username and password, and you want that token to be passed in each subsequent call in the cookie. We will use a cookie jar for this use case. 

Let’s see both the use cases one by one. 

## **Set cookie for one of the request**

This is the case where golang acts as an HTTP client. **AddCookie** method of **net/http** package can be used add a cookie. If we call this method for two different names and value,  then both that name and value will be added to the resultant cookie

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

**Output**

```
token=some_token
clicked=true
StatusCode: 200
```

We first need to create the instance of the cookie as below

[https://golang.org/src/net/http/cookie.go](https://golang.org/src/net/http/cookie.go)

```
cookie := &http.Cookie{
    Name:   "token",
    Value:  "some_token",
    MaxAge: 300,
}
```

Once the cookie is created then use the **AddCookie** method to add it to the HTTP request object

```
req.AddCookie(cookie)
```

In the above program, HTTP Client added two cookies with the below name-value pair.

*   token=some_token

*   clicked=true

Both these cookies will be sent in the call to [google.com](http://google.com).

## ****Set cookie and send that cookie for all requests to that domain****

HTTP client in golang lets you specify a **CookieJar** that can be used for storing and sending the cookies while making external HTTP requests. As the name suggests, think of it as a jar that contains cookies.

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

net/http provides a default cookie jar implementation that implements the above **CookieJar** interface. We will use it while initializing our net/http Client

[https://golang.org/pkg/net/http/cookiejar/#Jar](https://golang.org/pkg/net/http/cookiejar/#Jar)

You can also provide your custom cookie jar during the initialization of net/http Client struct which implements the above **CookieJar** interface.

HTTP client uses this jar in two ways

*   Adding cookies in this Jar. You can explicitly add cookies to this jar. The cookies will also be added to the jar if the server sends the Set-Cookies header in the response headers. All the cookies specified in the **Set-Cookie** header will be added. For more details around the Set-Cookie header please refer to this [link](https://golangbyexample.com/set-cookie-response-header/). This link contains all the details to understand the **Set-Cookie** header in golang.

*   For consulting this jar while making any external HTTP requests. It checks this jar to know what all cookies it needs to send for a particular domain

Let’s see an example where the client will add a cookie while making HTTP requests. This cookie will be sent in all subsequent requests to the same domain.

In this example, we will see how the client can set the cookie in the cookie jar. For that let’s first create a client. Here is the program for the same

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

To test the above code and to illustrate that cookies added to the cookie jar in the first call are indeed sent in the subsequent call as well, we need to create the server as well which will print the incoming cookies.

### **Server**

The server listens to port 8080 and has two APIs. These are the two APIs which is being hit by the client above.

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

This is all about setting cookies in golang. Hope you have liked the tutorial. Please share feedback in the comments.

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [cookie](https://golangbyexample.com/tag/cookie/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [set](https://golangbyexample.com/tag/set/)*