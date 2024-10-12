<!--yml
category: 未分类
date: 2024-10-13 06:38:29
-->

# Return 404 (Resource Not Found) Status Code in http response in Go (Golang)

> 来源：[https://golangbyexample.com/404-http-status-code-golang/](https://golangbyexample.com/404-http-status-code-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

http package of golang provides the status code constant which could be used to return different status codes- [https://golang.org/src/net/http/status.go](https://golang.org/src/net/http/status.go)

The same can also be used to return the 404 (Resource Not Found) http status code. The http 404 status code is defined by the below constant

```
http.StatusNotFound
```

In this article, we will also see how to return a JSON body along with the 404 (Resource Not Found) http status code

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
	w.WriteHeader(http.StatusNotFound)
	w.Header().Set("Content-Type", "application/json")
	resp := make(map[string]string)
	resp["message"] = "Resource Not Found"
	jsonResp, err := json.Marshal(resp)
	if err != nil {
		log.Fatalf("Error happened in JSON marshal. Err: %s", err)
	}
	w.Write(jsonResp)
	return
}
```

Here we are using the **WriteHeader** function to specify the 404 http status code and uses the **Write** function to also return the response bodyThe above code returns the below JSON request body back in response

```
{"message":"Resource Not Found"}
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
< HTTP/1.1 404 Not Found
< Date: Sat, 10 Jul 2021 06:18:12 GMT
< Content-Length: 32
< Content-Type: text/plain; charset=utf-8
< 
* Connection #0 to host localhost left intact
{"message":"Resource Not Found"}
```

As you can see from the output, it correctly returns the **404** status code along with the body.

You can also directly pass 404 to the **WriteHeader** function to send the 404 response.

```
w.WriteHeader(404)
```

This also works correctly. Try it out.

**net/http** package of golang also provides a **"NotFound"** handler that could be used to return a 404 for a particular API every time.

[https://golang.org/pkg/net/http/#NotFound](https://golang.org/pkg/net/http/#NotFound)

This handler function return 404 status and below response body

```
404 page not found
```

Below is the simple program for the same

```
package main

import (
	"net/http"
)

func main() {
	http.HandleFunc("/example", http.NotFound)
	http.ListenAndServe(":8080", nil)
}
```

In the above program, we simply specified the NotFound handler for **localhost:8080/example** API as below

```
http.HandleFunc("/example", http.NotFound)
```

Run the above program. It will start a server on 8080 port on your local machine. Now make the below curl call to the server

```
curl -v -X POST http://localhost:8080/example
```

```
* Connected to localhost (::1) port 8080 (#0)
> POST /example HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 404 Not Found
< Content-Type: text/plain; charset=utf-8
< X-Content-Type-Options: nosniff
< Date: Sat, 10 Jul 2021 06:23:41 GMT
< Content-Length: 19
< 
404 page not found
* Connection #0 to host localhost left intact
```

Also, check out our Golang advance tutorial Series - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [404](https://golangbyexample.com/tag/404/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*