<!--yml
category: 未分类
date: 2024-10-13 06:33:51
-->

# Set a timeout while making an HTTP request in Go (Golang)

> 来源：[https://golangbyexample.com/set-timeout-http-golang/](https://golangbyexample.com/set-timeout-http-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

**client** struct of HTTP package can be used to specify the timeout. While creating the **HTTP** client we can specify the value of the Timeout. An important thing to note about HTTP Client is that it is only created once and the same instance is used for making multiple HTTP requests. Below is the structure of **http.Client** struct

```
type Client struct {
	Transport RoundTripper
	CheckRedirect func(req *Request, via []*Request) error
	Jar CookieJar
	Timeout time.Duration
}
```

As you can see there is an option to specify the timeout for the **http.Client** which is the **Timeout** field

Now let’s see a working example of this

# **Program**

In the below program we are setting a timeout of 1 nanosecond to showcase that timeout is indeed happening while making the request.

```
package main
import (
    "fmt"
    "net/http"
    "os"
    "time"
)
func main() {
    client := &http.Client{
        Timeout: time.Nanosecond * 1,
    }
    _, err := client.Get("https://google.com")
    if os.IsTimeout(err) {
        fmt.Println("Timeout Happened")
    } else {
        fmt.Println("Timeout Did not Happened")
    }
}
```

**Output**

```
Timeout Happened
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*