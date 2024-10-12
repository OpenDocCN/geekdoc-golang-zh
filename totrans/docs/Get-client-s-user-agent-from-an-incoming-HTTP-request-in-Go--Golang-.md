<!--yml
category: 未分类
date: 2024-10-13 06:32:13
-->

# Get client’s user agent from an incoming HTTP request in Go (Golang)

> 来源：[https://golangbyexample.com/user-agent-http-golang/](https://golangbyexample.com/user-agent-http-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

User-agent in an incoming HTTP request is present in the headers of the request. In Go an incoming HTTP request is represented by the **http.Request** struct

[https://golang.org/src/net/http/request.go](https://golang.org/src/net/http/request.go)

The **http.Request** struct exposes the below method for getting the user-agent of the request

```
func (r *Request) UserAgent() string
```

This method needs to be called on the request object.

# **Example**

Let’s see the program for the same.

```
package main

import (
	"fmt"
	"net/http"
)

func main() {
	handler := http.HandlerFunc(handleRequest)
	http.Handle("/example", handler)
	http.ListenAndServe(":8080", nil)
}

func handleRequest(w http.ResponseWriter, r *http.Request) {

	userAgent := r.UserAgent()
	fmt.Printf("UserAgent:: %s", userAgent)
}
```

Run the above program and make the below API call. Also pass in the user-agent header

```
curl -v -X POST http://localhost:8080/example -H "user-agent: Mozialla -1.0"
```

**Output**

```
UserAgent:: Mozialla -1.0
```

In the above program, we are calling the **UesrAgent()** function on the request struct object and then printing it. It correctly prints the user agent

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*