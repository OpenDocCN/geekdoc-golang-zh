<!--yml
category: 未分类
date: 2024-10-13 06:31:28
-->

# Get request headers from an incoming HTTP request in Go (Golang)

> 来源：[https://golangbyexample.com/headers-http-request-golang/](https://golangbyexample.com/headers-http-request-golang/)

Note: Related Post

*   Set response headers for an incoming HTTP request in Go – [https://golangbyexample.com/set-resposne-headers-http-go/](https://golangbyexample.com/set-resposne-headers-http-go/)

*   Get response headers for an outgoing HTTP request in Golang- [https://golangbyexample.com/get-response-headers-making-go/](https://golangbyexample.com/get-response-headers-making-go/)

*   Set headers for an outgoing HTTP request in Go (Golang)– [https://golangbyexample.com/set-headers-http-request/](https://golangbyexample.com/set-headers-http-request/)

Now let’s look at how we can get request headers in an incoming HTTP request

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Using Header.Values method](#Using_HeaderValues_method "Using Header.Values method")
*   [Using Header.Get method](#Using_HeaderGet_method "Using Header.Get method")
*   [Directly Accessing the Header Struct](#Directly_Accessing_the_Header_Struct "Directly Accessing the Header Struct")
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

For eg if there are below headers in an incoming request

```
content-type: applcation/json
foo: bar1
foo: bar2
```

Then at server then headers will look like

```
map[string][]string {
  "Content-Type": {"application/json"},
  "Foo": {"bar1" , "bar2"}
}
```

Notice that:

*   **content-type** is converted to canonical form **Content-Type**

*   **content-type** header is represented is an array of string with 1 elements **application/json**

*   **foo** header is represented is an array of string with two elements **bar1** and **bar2**

Now we have seen how a header is represented in the request. Let’s see how we can get the header values given a key. Assume we have the below key-value pairs of headers

```
content-type: applcation/json
foo: bar1In the below example let's assume that variable r of type *http.Request type. Now let's see different ways of getting a header value.
foo: bar2
```

**Header** is present within the **http.Request** struct like as below.

```
type Request struct {
    Method string
    Header Header
    ...
}
```

In the below example let’s assume that variable **r** of type ***http.Request** type. Now let’s see different ways of getting a header value given a key

# **Using Header.Values method**

Used for accessing all values of a header given a key using the Values method. Below is the format of the method

```
func (h Header) Values(key string) []string
```

Notice that return type is a slice of string. It will return all the values associated with a given header key. Also the given key is converted to the canonical form first before fetching the values. It can be called as below

```
r.Header.Values(key)
```

For eg  for

*   **r.Header.Values(“content-type”)** it will output **“application/json”**  

*   **r.Header.Values(“foo”)** will output **[“bar1”, “bar2”]**

# **Using Header.Get method**

Used for accessing the first value of a header given a key. It is most common method used because generally only single value is associated with a header. Below is the format of the method.

```
func (h Header) Get(key string) string
```

Notice that the return type is just a string. It will return the first value associated with the given key. Also, the given key is converted to the canonical form first before fetching the values. It can be called as below

```
r.Header.Get(key)
```

For eg  for

*   **r.Header.Get(“content-type”)** it will output **“application/json”**

*   **r.Header.Get(“foo”)** will output **“bar1”**. Only the first value will be returned.

# **Directly Accessing the Header Struct**

Header map can also be directly accessed which is present within ***http.Request**

For eg

*   **r.Header** will output a map

```
map[Accept:[*/*] Content-Type:[application/json] Foo:[bar1 bar2]
```

*   **r.Header[“Content-Type”]** will output

```
[application/json]
```

*   **r.Header[“Foo”]** will output

```
[bar1 bar2]
```

Please note that when accessing Header map directly, we need to supply the key in canonical form only.

# **Example**

Let’s see a program illustrating all the above points

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

	fooVAlues := r.Header.Values("foo")
	fmt.Printf("r.Header.Values(\"foo\"):: %s\n\n", fooVAlues)

	contentType := r.Header.Get("content-type")
	fmt.Printf("r.Header.Get(\"content-type\"):: %s\n\n", contentType)

	fooFirstValue := r.Header.Get("foo")
	fmt.Printf("r.Header.Get(\"foo\"):: %s\n\n", fooFirstValue)

	headers := r.Header
	fmt.Printf("r.Headers:: %s\n\n", headers)
	fmt.Printf("r.Headers[\"Content-Type\"]:: %s\n\n", headers["Content-Type"])
	fmt.Printf("r.Headers[\"Foo\"]:: %s", headers["Foo"])
}
```

In above program we started a server which is listening on port 8080\. We also defined a URL on that endpoint. Run this server and make the below API call.

```
curl -v -X POST http://localhost:8080/example -H "content-type: application/json" -H "foo: bar1" -H "foo: bar2"
```

After running this API check the output in the terminal. It will output. You can check the output. It is exactly as we have discussed

```
r.Header.Values("foo"):: [bar1 bar2]

r.Header.Get("content-type"):: application/json

r.Header.Get("foo"):: bar1

r.Headers:: map[Accept:[*/*] Content-Type:[application/json] Foo:[bar1 bar2] User-Agent:[curl/7.54.0]]

r.Headers["Content-Type"]:: [application/json]

r.Headers["Foo"]:: [bar1 bar2]
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*