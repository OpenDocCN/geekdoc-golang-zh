<!--yml
category: 未分类
date: 2024-10-13 06:38:42
-->

# Return 200 (StatusOK) Status Code in HTTP response in Go (Golang) -P

> 来源：[https://golangbyexample.com/200-http-status-response-golang/](https://golangbyexample.com/200-http-status-response-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

**net/http** package of golang provides the status code constants which could be used to return different status codes- [https://golang.org/src/net/http/status.go](https://golang.org/src/net/http/status.go)

The same can also be used to return the 200 (StatusOK) HTTP status code.  The HTTP 200 status code is defined by the below constant

```
http.StatusOK
```

In this article, we will also see how to return a JSON body along with the 200 (StatusOK) Status Code

## **Program**

Below is the program for the same

```
package main

import (
	"encoding/json"
	"log"
	"net/http"
)

func main() {
	handler := http.HandlerFunc(handleRequest)
	http.Handle("/example", handler)
	http.ListenAndServe(":8080", nil)
}

func handleRequest(w http.ResponseWriter, r *http.Request) {
	w.WriteHeader(http.StatusOK)
	w.Header().Set("Content-Type", "application/json")
	resp := make(map[string]string)
	resp["message"] = "Status OK"
	jsonResp, err := json.Marshal(resp)
	if err != nil {
		log.Fatalf("Error happened in JSON marshal. Err: %s", err)
	}
	w.Write(jsonResp)
	return
}
```

Here we are using the **WriteHeader** function to specify the 200 http status code and uses the **Write** function to also return the response bodyThe above code returns the below JSON request body back in response

```
{"message":"Status OK"}
```

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
< Date: Sat, 10 Jul 2021 09:42:59 GMT
< Content-Length: 23
< Content-Type: text/plain; charset=utf-8
< 
* Connection #0 to host localhost left intact
{"message":"Status OK"}
```

As you can see from the output, it will correctly return the 200 status code along with the body.

You can also directly pass 200 to the WriteHeader function to send the 200 response.

```
w.WriteHeader(200)
```

This also works correctly. Try it out.

Also, check out our Golang advance tutorial Series - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [200](https://golangbyexample.com/tag/200/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*