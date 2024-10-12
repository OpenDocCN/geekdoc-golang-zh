<!--yml
category: 未分类
date: 2024-10-13 06:39:17
-->

# Return plain text body in HTTP response in Go (Golang)

> 来源：[https://golangbyexample.com/plain-text-response-body-golang/](https://golangbyexample.com/plain-text-response-body-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *## **Overview**

**Write** method of the ResponseWriter interface in **net/http** package can be used to set the **text/plain** body in an HTTP response

In GO a response is represented by the **ResponseWriter** Interface.  Here is the link to the interface –[https://golang.org/pkg/net/http/#ResponseWriter](https://golang.org/pkg/net/http/#ResponseWriter)

ResponseWriter interface is used by an HTTP handler to construct an HTTP response. It provides three functions to set the response parameters

*   Header – For writing response header

*   Write([]byte) – For writing response body

*   WriteHeader(statusCode int) – For writing the http status code

**Write** function can be used to set the response body. It takes a slice of bytes as input. Also, there is a **Header** function. This function can be used to set the content type of the response body using the Content-Type header. For eg in the case of the text/plain response body, we need to set the Content-Type header as **“text/plain”.**

```
w.Header().Set("Content-Type", "text/plain")
```

Also, note that **WriteHeader** function can be used to set the HTTP status code for the response

## **Example**

Let’s see an example of sending http status code and **text/plain** response body

Below is the program for the same

```
package main

import (
	"net/http"
)

func main() {
	handler := http.HandlerFunc(handleRequest)
	http.Handle("/example", handler)
	http.ListenAndServe(":8080", nil)
}

func handleRequest(w http.ResponseWriter, r *http.Request) {
	w.WriteHeader(http.StatusOK)
	w.Header().Set("Content-Type", "application/text")
	w.Write([]byte("Success"))
	return
}
```

We use the **Write** function to return the text/plain response body. The above code returns the below **text/plain** body back in response

```
Success
```

Also, we are using the **WriteHeader** function to specify the **200** http status code.  We are also setting the correct header

Run the above program. It will start a server on 8080 port on your local machine. Now make the below curl call to the server

```
curl -v -X POST http://localhost:8080/example
```

Below will be the output

```
* Connected to localhost (::1) port 8080 (#0)
> POST /example HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Date: Sat, 10 Jul 2021 19:01:56 GMT
< Content-Length: 7
< Content-Type: text/plain; charset=utf-8
< 
* Connection #0 to host localhost left intact
Success
```

As you can see from the output, it will correctly return the **200** status code along with the **text/plain** body. Also, the **Content-Type** response header is set to **text/plain**

Also, check out our Golang advance tutorial Series - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*