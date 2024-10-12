<!--yml
category: 未分类
date: 2024-10-13 06:32:02
-->

# Set request headers for an outgoing HTTP request in Go (Golang)

> 来源：[https://golangbyexample.com/set-headers-http-request/](https://golangbyexample.com/set-headers-http-request/)

Note: Related Post

*   Get headers from an incoming HTTP request in Go – [https://golangbyexample.com/headers-http-request-golang/](https://golangbyexample.com/headers-http-request-golang/)

*   Set response headers for an incoming HTTP request in Go (Golang) – [https://golangbyexample.com/set-resposne-headers-http-go/](https://golangbyexample.com/set-resposne-headers-http-go/)

*   Get response headers for an outgoing HTTP request in Golang- [https://golangbyexample.com/get-response-headers-making-go/](https://golangbyexample.com/get-response-headers-making-go/)

Now let’s look at how we can set headers while making a HTTP request

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Using r.Header.Add() method](#Using_rHeaderAdd_method "Using r.Header.Add() method")
*   [Using r.Header.Set method](#Using_rHeaderSet_method "Using r.Header.Set method")
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

For eg if below headers are set in the outgoing request from a client then

```
user-agent: goapplication
foo: bar1
foo: bar2
```

Then at server the headers will look like

```
User-Agent: goapplication
Foo: bar1
Foo: bar2
```

Notice that:

*   **user-agent** is converted to canonical form **User-Agent**
*   **foo** header is converted to **Foo**. Also note that there will be two headers that will be set in the request with the same key which is **foo**. The values will be **bar1** and **bar2**

Now we have seen how a header is represented in the request. Let’s see how we can set the request headers values when making HTTP requests. Assume we have the below key value pairs of headers which we have to set in the HTTP request

```
user-agent: goapplication
foo: bar1
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

In the below example let’s assume that variable **r** of type ***http.Request** type. Now let’s see different ways of setting a header

# **Using r.Header.Add() method**

Below is the signature of the Add method

```
func (h Header) Add(key, value string)
```

This method is used to add the key value pair to the request header. We have already seen above that Header value can be array as well. So this method appends to the existing values that might already have been associated with the key.  Also the key will be converted to canonical form. For example if we add the foo header two times with different values

```
r.Header.Add("foo", "bar1")
r.Header.Add("foo", "bar2")
```

Then **foo** header will be set to **[“bar1”, “bar2”].** Also **foo** will become **Foo** before making the http call

# **Using r.Header.Set method**

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
    req.Header.Set("user-agent", "golang application")
    req.Header.Add("foo", "bar1")
    req.Header.Add("foo", "bar2")
    response, err := client.Do(req)
    if err != nil {
        return fmt.Errorf("Got error %s", err.Error())
    }
    defer response.Body.Close()
    return nil
}
```

Note in the above example how we are setting the headers while making the HTTP call

```
req.Header.Set("user-agent", "goapplication")
req.Header.Add("foo", "bar1")
req.Header.Add("foo", "bar2")
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*