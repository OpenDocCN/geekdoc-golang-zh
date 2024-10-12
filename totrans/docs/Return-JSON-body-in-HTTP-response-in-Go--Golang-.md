<!--yml
category: 未分类
date: 2024-10-13 06:39:02
-->

# Return JSON body in HTTP response in Go (Golang)

> 来源：[https://golangbyexample.com/json-response-body-http-go/](https://golangbyexample.com/json-response-body-http-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *## **Overvie**w

**Write** method of the ResponseWriter interface in **net/http** package can be used to set the JSON body in an HTTP response

In GO a response is represented by the **ResponseWriter** Interface.  Here is the link to the interface –

[https://golang.org/pkg/net/http/#ResponseWriter](https://golang.org/pkg/net/http/#ResponseWriter)

ResponseWriter interface is used by an HTTP handler to construct an HTTP response. It provides three functions to set the response parameters

*   Header – For writing response header

*   Write([]byte) – For writing response body

*   WriteHeader(statusCode int) – For writing the http status code

**Write** function can be used to set the response body. It takes a slice of bytes as input. Also, there is a **Header** function. This function can be used to set the content type of the response body using the Content-Type header. For eg in the case of the JSON response body, we need to set the Content-Type header as **“application/json”.**

```
w.Header().Set("Content-Type", "application/json")
```

Also, note that **WriteHeader** function can be used to set the HTTP status code for the response

## **Example**

Let’s see an example of sending http status code and JSON response body

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

In the above program, this is how we create a JSON response. We use the **json.Marshal** function to convert the **map[string]string** into json bytes.

```
resp := make(map[string]string)
resp["message"] = "Status Created"
jsonResp, err := json.Marshal(resp)
if err != nil {
	log.Fatalf("Error happened in JSON marshal. Err: %s", err)
}
w.Write(jsonResp)
```

It then uses the **Write** function to return the JSON response body. The above code returns the below JSON response body back in response

```
{"message":"Status Created"}
```

Also, we are using the **WriteHeader** function to specify the 201 http status code.

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

As you can see from the output, it will correctly return the **201** status code along with the JSON body.

Also, check out our Golang advance tutorial Series - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*