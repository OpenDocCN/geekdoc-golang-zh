<!--yml
category: 未分类
date: 2024-10-13 06:39:10
-->

# Return 202 (StatusAccepted) in HTTP response in Go (Golang)

> 来源：[https://golangbyexample.com/202-status-http-response-go/](https://golangbyexample.com/202-status-http-response-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

**net/http** package of golang provides the status code constants which could be used to return different status codes- [https://golang.org/src/net/http/status.go](https://golang.org/src/net/http/status.go)

The same can also be used to return the 202 (StatusAccepted) HTTP status code.  The HTTP 202 status code is defined by the below constant.

```
http.StatusAccepted
```

In this article, we will also see how to return a JSON body along with the 202(StatusAccepted) Status Code

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
	w.WriteHeader(http.StatusAccepted)
	w.Header().Set("Content-Type", "application/json")
	resp := make(map[string]string)
	resp["message"] = "Status Accepted"
	jsonResp, err := json.Marshal(resp)
	if err != nil {
		log.Fatalf("Error happened in JSON marshal. Err: %s", err)
	}
	w.Write(jsonResp)
	return
}
```

Here we are using the **WriteHeader** function to specify the 202 http status code and uses the **Write** function to also return the response body. The above code returns the below JSON request body back in response

```
{"message":"Status Accepted"}
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
< HTTP/1.1 202 Accepted
< Date: Sat, 10 Jul 2021 17:42:27 GMT
< Content-Length: 29
< Content-Type: text/plain; charset=utf-8
< 
* Connection #0 to host localhost left intact
{"message":"Status Accepted"}
```

As you can see from the output, it will correctly return the 202 status code along with the body.

You can also directly pass 202 to the WriteHeader function to send the 202 response.

```
w.WriteHeader(202)
```

This also works correctly. Try it out.

Also, check out our Golang advance tutorial Series - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*