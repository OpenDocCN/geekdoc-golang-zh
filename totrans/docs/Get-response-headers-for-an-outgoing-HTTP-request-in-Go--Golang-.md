<!--yml
category: 未分类
date: 2024-10-13 06:32:08
-->

# Get response headers for an outgoing HTTP request in Go (Golang)

> 来源：[https://golangbyexample.com/get-response-headers-making-go/](https://golangbyexample.com/get-response-headers-making-go/)

Note: Related Post

*   Get request headers from an incoming HTTP request in Golang – [https://golangbyexample.com/headers-http-request-golang/](https://golangbyexample.com/headers-http-request-golang/)

*   Set response headers for an incoming HTTP request in Golang – [https://golangbyexample.com/set-resposne-headers-http-go/](https://golangbyexample.com/set-resposne-headers-http-go/)

*   Set request headers for an outgoing HTTP request in Golang- [https://golangbyexample.com/set-headers-http-request/](https://golangbyexample.com/set-headers-http-request/)

Now let’s look at how we can get headers while making an HTTP request

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

*   key being represented in canonical form. Canonical form means that the first character and any character following a hyphen is in uppercase. All other characters are in lowercase. Example of canonical forms are

```
Content-Type
Accept-Encoding
```

Value is represented as a slice of string. Why an array of strings? Because it is perfectly okay to two headers with the same key and different values in a request. Both the values will be collected in the slice.

Response in http request is represented by **http.Response** struct.

[https://golang.org/src/net/http/response.go](https://golang.org/src/net/http/response.go)

There is a **Header** field defined in the **http.Response** struct as below. It contains the response headers associated with the response

```
type Response interface {
    Header() Header
    ...
}
```

In the below example let’s assume that variable **w** is of type **http.Response**. Now let’s see different ways of getting a response header. Also, assume that we have below response headers for now in an outgoing request

```
User-Agent: application/json
Foo: bar1
Foo: bar2
```

# **Using Header.Values method**

Used for accessing all values of a header given a key using the Values method. Below is the format of the method

```
func (h Header) Values(key string) []string
```

Notice that return type is a slice of string. It will return all the values associated with a given header key. Also the given key is converted to the canonical form first before fetching the values. It can be called as below

```
w.Header.Values(key)
```

For eg  for

*   **w.Header.Values(“content-type”)** will output **[“application/json” ]**

*   **w.Header.Values(“foo”)** will output **[“bar1”, “bar2”]**

## **Using Header.Get method**

Used for accessing the first value of a header given a key. It is most common method used because generally only single value is associated with a header. Below is the format of the method.

```
func (h Header) Get(key string) string
```

Notice that the return type is just a string. It will return the first value associated with the given key. Also, the given key is converted to the canonical form first before fetching the values. It can be called as below

```
w.Header.Get(key)
```

For eg  for

*   **w.Header.Get(“content-type”)** it will output **“application/json”**

*   **w.Header.Get(“foo”)** will output **“bar1”**. Only the first value will be returned.

# **Directly Accessing the Header Struct**

Header map can also be directly accessed which is present within *http.Response

For eg

*   **w.Header** will output a map

```
map[Accept:[*/*] Content-Type:[application/json] Foo:[bar1 bar2]
```

*   **w.Header[“Content-Type”]** will output

```
[application/json]
```

*   **w.Header[“Foo”]** will output

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
	"time"
)

func main() {
	call("https://google.com", "GET")
}

func call(url, method string) error {
	client := &http.Client{
		Timeout: time.Second * 10,
	}
	req, err := http.NewRequest(method, url, nil)
	if err != nil {
		return fmt.Errorf("Got error %s", err.Error())
	}
	res, err := client.Do(req)
	if err != nil {
		return fmt.Errorf("Got error %s", err.Error())
	}
	defer res.Body.Close()

	contentTypeValues := res.Header.Values("content-type")
	fmt.Printf("res.Header.Values(\"content-type\"):: %s\n\n", contentTypeValues)

	contentType := res.Header.Get("content-type")
	fmt.Printf("res.Header.Get(\"content-type\"):: %s\n\n", contentType)

	headers := res.Header
	fmt.Printf("res.Headers:: %s\n\n", headers)
	fmt.Printf("res.Headers[\"Content-Type\"]:: %s\n\n", headers["Content-Type"])
	return nil

}
```

After running the above program,  check the output in the terminal. It is exactly as we have discussed. Below will be the output

```
res.Header.Values("content-type"):: 

res.Header.Get("content-type"):: text/html; charset=ISO-8859-1

res.Headers:: map[Alt-Svc:[h3-29=":443"; ma=2592000,h3-T051=":443"; ma=2592000,h3-Q050=":443"; ma=2592000,h3-Q046=":443"; ma=2592000,h3-Q043=":443"; ma=2592000,quic=":443"; ma=2592000; v="46,43"] Cache-Control:[private, max-age=0] Content-Type: Date:[Thu, 10 Dec 2020 16:38:03 GMT] Expires:[-1] P3p:[CP="This is not a P3P policy! See g.co/p3phelp for more info."] Server:[gws] Set-Cookie:[1P_JAR=2020-12-10-16; expires=Sat, 09-Jan-2021 16:38:03 GMT; path=/; domain=.google.com; Secure NID=204=w6zf-xFyVywx7QaClDZuQ5N-Yc-4HqKWBS-JXWp2Tat9kmq0BRsanM35PJHiM2iEn4TbP2HcTUd0KkIuMuIW7xFewD5un2_mc0O4fm2IXzrQyRmPWHJSeQJUUVb0-_lIfJgSnGmJm2MptRsd2egrPsbZJQBZWd97o7KlFBI3CIE; expires=Fri, 11-Jun-2021 16:38:03 GMT; path=/; domain=.google.com; HttpOnly] X-Frame-Options:[SAMEORIGIN] X-Xss-Protection:[0]]

res.Headers["Content-Type"]:: 
```

Notice that

```
res.Header.Values("content-type"):
```

outputs an array

 ````while    ``` res.Header.Get("content-type") ```    outputs    ``` text/html; charset=ISO-8859-1 ```    Also    ``` res.Headers ```    outputs the entire header map. Notice that each key is in the canonical format    ``` map[Alt-Svc:[h3-29=":443"; ma=2592000,h3-T051=":443"; ma=2592000,h3-Q050=":443"; ma=2592000,h3-Q046=":443"; ma=2592000,h3-Q043=":443"; ma=2592000,quic=":443"; ma=2592000; v="46,43"] Cache-Control:[private, max-age=0] Content-Type: Date:[Thu, 10 Dec 2020 16:38:03 GMT] Expires:[-1] P3p:[CP="This is not a P3P policy! See g.co/p3phelp for more info."] Server:[gws] Set-Cookie:[1P_JAR=2020-12-10-16; expires=Sat, 09-Jan-2021 16:38:03 GMT; path=/; domain=.google.com; Secure NID=204=w6zf-xFyVywx7QaClDZuQ5N-Yc-4HqKWBS-JXWp2Tat9kmq0BRsanM35PJHiM2iEn4TbP2HcTUd0KkIuMuIW7xFewD5un2_mc0O4fm2IXzrQyRmPWHJSeQJUUVb0-_lIfJgSnGmJm2MptRsd2egrPsbZJQBZWd97o7KlFBI3CIE; expires=Fri, 11-Jun-2021 16:38:03 GMT; path=/; domain=.google.com; HttpOnly] X-Frame-Options:[SAMEORIGIN] X-Xss-Protection:[0]] ```  *   [golang](https://golangbyexample.com/tag/golang/)````*