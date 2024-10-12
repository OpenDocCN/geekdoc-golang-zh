<!--yml
category: 未分类
date: 2024-10-13 06:31:57
-->

# Set response headers for an incoming HTTP request in Go (Golang)

> 来源：[https://golangbyexample.com/set-resposne-headers-http-go/](https://golangbyexample.com/set-resposne-headers-http-go/)

Note: Related Post

*   Get headers from an incoming HTTP request in Go – [https://golangbyexample.com/headers-http-request-golang/](https://golangbyexample.com/headers-http-request-golang/)

*   Get response headers for an outgoing HTTP request in Golang- [https://golangbyexample.com/get-response-headers-making-go/](https://golangbyexample.com/get-response-headers-making-go/)

*   Set headers for an outgoing HTTP request in Go (Golang) – [https://golangbyexample.com/set-headers-http-request/](https://golangbyexample.com/set-headers-http-request/)

Now let’s look at how we can set response headers in an incoming HTTP request

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Using w.Header().Add() method](#Using_wHeaderAdd_method "Using w.Header().Add() method")
*   [Using w.Header().Set method](#Using_wHeaderSet_method "Using w.Header().Set method")
*   [Example](#Example "Example")*  *# **Overview**

Below is the format in which a Header is represented in go.

```
type Header map[string][]string
```

So the header is a key value pair with

*   key being represented in canonical form. Canonical form means that first character and any character following a hyphen is in uppercase. All other characters is in lowercase. Example of canonical forms are

```
Content-Type
Accept-Encoding
```

*   Value is represented as a slice of string. Why array of strings? Because it is perfectly okay to two headers with same key and different values in a request. Both the values will be collected in the slice.

For eg if below headers are set in the outgoing response from a server then

```
content-type: applcation/json
foo: bar1
foo: bar2
```

Then at client the headers will look like

```
Content-Type: applcation/json
Foo: bar1
Foo: bar2
```

Notice that:

*   **content-type** is converted to canonical form **Content-Type**

*   **foo** header is converted to **Foo**. Also note that there will be two headers in the response with the same key which is **foo**. The values will be **bar1** and **bar2**

Now we have seen how a header is represented in the request. Let’s see how we can set the response headers values. Assume we have the below key value pairs of headers which we have to set in the response headers

```
content-type: applcation/json
foo: bar1
foo: bar2
```

There is a **Header** method defined in the **http.ResponseWriter** interface as below. It returns the Header associated with the response.

```
type ResponseWriter interface {
    Header() Header
    ...
}
```

In the below example let’s assume that variable **w** is of type **http.ResponseWriter** . Now let’s see different ways of setting a header

# **Using w.Header().Add() method**

Below is the signature of the Add method

```
func (h Header) Add(key, value string)
```

This method is used to add the key value pair to the response header. We have already seen above that Header value can be array as well. So this method appends to the existing values that might already have been associated with the key.  Also the key will be converted to canonical form.

For example if we add the foo header two times with different values

```
w.Header().Add("foo", "bar1")
w.Header().Add("foo", "bar2")
```

Then **foo** header will be set to **[“bar1”, “bar2”].** Also **foo** will become **Foo** when sending response to the client.

# **Using w.Header().Set method**

Below is the signature of the function

```
func (h Header) Set(key, value string)
```

It can be used to set the header entries associated with the given key. Unlike the **Add** method, this method will replace any existing values associated with the key. Also the key will be converted to canonical form.

# **Example**

Let’s see a program illustrating all the above points

```
package main

import (
	"encoding/json"
	"net/http"
)

func main() {
	handler := http.HandlerFunc(handleRequest)
	http.Handle("/example", handler)
	http.ListenAndServe(":8085", nil)
}

func handleRequest(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("content-type", "application/json")

	w.Header().Add("foo", "bar1")
	w.Header().Add("foo", "bar2")

	resp := make(map[string]string)
	resp["message"] = "success"
	jsonResp, _ := json.Marshal(resp)
	w.Write(jsonResp)
}
```

In above program we started a server which is listening on port 8085\. Also we are setting the **content-type** header as below

```
w.Header().Set("content-type", "application/json")
```

Also we are setting the **foo** header as below

```
w.Header().Add("foo", "bar1")
w.Header().Add("foo", "bar2")
```

We also defined a URL on that endpoint. Run this server and make the below API call.

```
curl -v -X POST http://localhost:8085/example
```

After running this API check the response headers in the terminal. It is exactly as we have discussed. Also each of the header key is converted to its canonical form.

```
< HTTP/1.1 200 OK
< Content-Type: application/json
< Foo: bar1
< Foo: bar2
< Content-Length: 21
< 
* Connection #0 to host localhost left intact
{"message":"success"}
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*