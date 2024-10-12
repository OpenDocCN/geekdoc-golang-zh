<!--yml
category: 未分类
date: 2024-10-13 06:02:36
-->

# net/http package get Query Params in Go(Golang)

> 来源：[https://golangbyexample.com/net-http-package-get-query-params-golang/](https://golangbyexample.com/net-http-package-get-query-params-golang/)

**Note:** If you are interested in learning Golang, then for that we have a golang comprehensive tutorial series. Do check it out – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/). Now let’s see current tutorial

Often in the context of an HTTP request, it is required to fetch the query params which are sent as part of the request. A particular query param key value can have one or multiple values.

*   In below example **filters** have one value which is [“color”]

```
http://localhost:8080/products?filters=color
```

*   In below example **filters** have multiple value which is [“color”, “price”, “brand”]. Notice how multiple values are defined

```
http://localhost:8080/products?filters=red&filters=color&filters=price&filters=brand
```

Let’s explore two methods of getting these query params

# 1.**Using reqeust.URL.Query()**

The query params lie in the **URL** itself. We get Query params via r.URL.Query() which returns **Values** which is of map[string][]string. There are two cases:

**1.1 When a particular key contains multiple values in a query params. For example, see below request-**

```
http://localhost:8080/products?filters=red&filters=color&filters=price&filters=brand
```

**Code:**

```
package main

import (
    "fmt"
    "net/http"
    "strings"
)

func main() {
    getProductsHandler := http.HandlerFunc(getProducts)
    http.Handle("/products", getProductsHandler)
    http.ListenAndServe(":8080", nil)
}

func getProducts(w http.ResponseWriter, r *http.Request) {
    query := r.URL.Query()
    filters, present := query["filters"] //filters=["color", "price", "brand"]
    if !present || len(filters) == 0 {
        fmt.Println("filters not present")
    }
    w.WriteHeader(200)
    w.Write([]byte(strings.Join(filters, ",")))
}
```

**Output:**

```
color,price,brand 
```

**1.2 When a particular key contains a single value in query params. For example, see below request**

```
http://localhost:8080/products?filters=color
```

When we know that a particular key has only one value in the query params, then r.URL.Query().Get(keyName) can also be used. Get function will get the first value associated with the key. If you want to all values then the map has to be directly accessed as we did in the above program. In the below program, we are using Get() on the r.URL.Query() and it returns a single value

```
package main

import (
    "net/http"
)

func main() {
    getProductsHandler := http.HandlerFunc(getProducts)
    http.Handle("/products", getProductsHandler)
    http.ListenAndServe(":8080", nil)
}

func getProducts(w http.ResponseWriter, r *http.Request) {
    query := r.URL.Query()
    filters := query.Get("filters") //filters="color"
    w.WriteHeader(200)
    w.Write([]byte(filters))
}
```

**Output:**

```
color
```

# **2.Using request.Form**

The query params lie in the **URL** itself. We get Query params via r.URL.Query() which returns **values** which is of map[string][]string. There are two cases:

**2.1** **When a particular key contains multiple values in a query params. For example, see below request**

```
http://localhost:8080/products?filters=red&filters=color&filters=price&filters=brand
```

**Code:**

```
package main

import (
    "fmt"
    "net/http"
    "strings"
)

func main() {
    getProductsHandler := http.HandlerFunc(getProducts)
    http.Handle("/products", getProductsHandler)
    http.ListenAndServe(":8080", nil)
}

func getProducts(w http.ResponseWriter, r *http.Request) {
    r.ParseForm()
    filters, present := r.Form["filters"] //filters=["color", "price", "brand"]
    if !present || len(filters) == 0 {
        fmt.Println("filters not present")
    }
    w.WriteHeader(200)
    w.Write([]byte(strings.Join(filters, ",")))
}
```

**Output:**

```
color,price,brand
```

An important point to be noted:

*   While trying to get query param value via r.Form , be careful that in case of POST and PUT request, body parameters will take precedence over URL query string values i.e, if there is a key say **X** present in both form body (=”a”) and in query param(=”b”).  Then on calling r.Form[“X”] it will return [“a”] and not [“b”]

**2.2 When a particular key contains a single value in query params. For example, see below request**

```
http://localhost:8080/products?filters=color
```

When we know that a particular key has only one value in the query params, then r.FormValue(keyName) can also be used. FormValue function will get the first value associated with the key. If you want to all values then request. The form map has to be directly accessed as we did in the above program. In the below program, we are using FormValue() function and it returns a single value.

```
package main

import (
    "net/http"
)

func main() {
    getProductsHandler := http.HandlerFunc(getProducts)
    http.Handle("/products", getProductsHandler)
    http.ListenAndServe(":8080", nil)
}

func getProducts(w http.ResponseWriter, r *http.Request) {
    filters := r.FormValue("filters") //filters=["color"]
    w.WriteHeader(200)
    w.Write([]byte(filters))
}
```

**Output:**

```
color
```

An important point to be noted:

*   While trying to get query param value via r.FormValue() , be careful that in case of POST and PUT request, body parameters will take precedence over URL query string values i.e, if there is a key say **X** present in both form body (=”a”) and in query param(=”b”).  Then on calling r.Form[“X”] it will return [“a”] and not [“b”]

*   [go](https://golangbyexample.com/tag/go/)*   [http](https://golangbyexample.com/tag/http/)*   [net http](https://golangbyexample.com/tag/net-http/)*   [query params](https://golangbyexample.com/tag/query-params/)