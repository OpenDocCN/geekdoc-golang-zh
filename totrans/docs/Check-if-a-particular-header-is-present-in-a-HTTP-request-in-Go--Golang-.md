<!--yml
category: 未分类
date: 2024-10-13 06:31:33
-->

# Check if a particular header is present in a HTTP request in Go (Golang)

> 来源：[https://golangbyexample.com/header-present-http-golang/](https://golangbyexample.com/header-present-http-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")*  *# **Overview**

Below is the format in which a Header is represented in go.

```
type Header map[string][]string
```

So internally header is a key value map with

*   key being represented in canonical form. Canonical form means that first character and any character following a hyphen is in uppercase. All other characters is in lowercase. Example of canonical forms are

```
Content-Type
Accept-Encoding
```

*   Value is represented as an array of string. Why array of strings? Because it is perfectly okay to two headers with same key and different values in a request. Both the values will be collected in the array.

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

And since header is a map, we can use the property of a map to check if a particular key is present or not. Below is the format to check if a key exist in the map

```
val, ok := mapName[key]
```

There are two cases

*   If the key exists **val **variable be the value of the key in the map and **ok **variable will be true

*   If the key doesn’t exist **val** variable will be default zero value of value type and **ok **variable will be false

Let’s see an example

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
    headers := r.Header
    val, ok := headers["Content-Type"]
    if ok {
        fmt.Printf("Content-Type key header is present with value %s\n", val)
    } else {
        fmt.Println("Content-Type key header is not present")
    }
    val, ok = headers["someKey"]
    if ok {
        fmt.Printf("someKey key header is present with value %s\n", val)
    } else {
        fmt.Println("someKey key header is not present")
    }
}
```

In above program we started a server which is listening on port 8080\. We also defined a URL on that endpoint. Run this server and make the below API call.

```
curl -v -X POST http://localhost:8080/example -H "content-type: application/json" 
```

After running this API check the output in the terminal. It will output. You can check the output. content-type header is present hence it outputs its value. someKey header is not present hence it prints that is no present. Also note that while accessing the header map we need to enter the key in the canonical form only. For example even though the supplied header in the curl is

```
content-type
```

but while accessing the header map in the code we do like below i.e using key in canonical form **Content-Type**

```
val, ok := headers["Content-Type"]
```

In case we only want to check if a key is present and val is not needed, then blank identifier i.e “_” can be used in place of val.

```
_, ok = employeeSalary["Sam"]
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*