<!--yml
category: 未分类
date: 2024-10-13 06:38:57
-->

# How to set HTTP status code in response in Go (Golang)

> 来源：[https://golangbyexample.com/set-http-status-code-golang/](https://golangbyexample.com/set-http-status-code-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *## **Overview**

**WriteHeader** method of the ResponseWriter interface in **net/http** package can be used to return the status code from a golang server.

In GO a response is represented by the **ResponseWriter** Interface.  Here is the link to the interface –

[https://golang.org/pkg/net/http/#ResponseWriter](https://golang.org/pkg/net/http/#ResponseWriter)

ResponseWriter interface is used by an HTTP handler to construct an HTTP response. It provides three functions to set the response parameters

*   Header – For writing response header

*   Write([]byte) – For writing response body

*   WriteHeader(statusCode int) – For writing the http status code

As you can see **WriteHeader** function takes statusCode as input and that status code is sent in the HTTP response. While **Write** function can be used to set the response body. It is to be noted that if **WriteHeader** is not called explicitly then the call to **Write** function will call the **WriteHeader** function internally with status code 200 which is StatusOk.

## **Example**

Let’s see an example of sending http status code and body

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
	w.WriteHeader(http.StatusCreated)
	w.Header().Set("Content-Type", "application/json")
	resp := make(map[string]string)
	resp["message"] = "Status Created"
	jsonResp, err := json.Marshal(resp)
	if err != nil {
		log.Fatalf("Error happened in JSON marshal. Err: %s", err)
	}
	w.Write(jsonResp)
	return
}
```

Here we are using the **WriteHeader** function to specify the 201 http status code. Similarly, we can send pass any of the status codes listed here to the **WriteHeader** function

[https://golang.org/src/net/http/status.go](https://golang.org/src/net/http/status.go)

Also it uses the **Write** function to return the response body. The above code returns the below JSON request body back in response

```
{"message":"Status Created"}
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
< HTTP/1.1 201 Created
< Date: Sat, 10 Jul 2021 10:40:33 GMT
< Content-Length: 28
< Content-Type: text/plain; charset=utf-8
< 
* Connection #0 to host localhost left intact
{"message":"Status Created"}
```

As you can see from the output, it will correctly return the **201** status code along with the body.

You can also directly pass 201 to the WriteHeader function to send the 201 response.

```
w.WriteHeader(201)
```

Try it out, it will work.

We mentioned that we don't call WriteHeader explicitly then the call to **Write** function will call the WriteHeader function internally with status code 200 which is StatusOk. Let's see an example of that-

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
	w.Header().Set("Content-Type", "application/json")
	resp := make(map[string]string)
	resp["message"] = "Success"
	jsonResp, err := json.Marshal(resp)
	if err != nil {
		log.Fatalf("Error happened in JSON marshal. Err: %s", err)
	}
	w.Write(jsonResp)
	return
}
```

See the code above. We haven't called the **WriteHeader** function anywhere. The program therefore should send the status code 200 by default.

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
< Content-Type: application/json
< Date: Sat, 10 Jul 2021 16:24:11 GMT
< Content-Length: 21
< 
* Connection #0 to host localhost left intact
{"message":"Success"}
```

As you can notice from the output that it returns the 200 status code.

Also, check out our Golang advance tutorial Series - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*