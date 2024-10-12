<!--yml
category: 未分类
date: 2024-10-13 06:32:33
-->

# HTTP client or Send x-www-form-urlencoded request body in Go (Golang)

> 来源：[https://golangbyexample.com/http-client-urlencoded-body-go/](https://golangbyexample.com/http-client-urlencoded-body-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

The **applcation/x-www-form-urlencoded** content-type request body is like a giant query string. Similar to the query string in a URI it is a key-value pair having the below format.

```
key1=value1&key2=value21&key2=value22&key3=value3
```

where there are below key-value pairs

*   key1, value1

*   key2 has two values i.e value21 and value22.

*   key3, value3

Each key-value pair is separated by **&** in case of multiple values for the same key there will be two entries of that key-value pair. Also, each key and value is URLencoded similar to the query string.

Now the question that might be coming to the mind is if **x-www-form-urlencoded** is just like query string then why does it exist. The reason is that the query string is part of the URI and since there is a limit on the length of the URI, you can send a limited number of key-value pairs in the query string.  While there is no limit for the length of the **x-www-form-urlencoded** request body. However, it is limited by the max request body size allowed by the server which is generally 10MB for most of the servers. Now let’s see how we can parse the **x-www-form-urlencoded** in golang

So basically since the x-www-form-urlencoded body is nothing but like query string, it is represented in golang using **url.Values**. [https://golang.org/pkg/net/url/#Values](https://golang.org/pkg/net/url/#Values)

**url.Values** is nothing but a map whose

*   key is a string 

*   value is a slice of string

It is represented like below

```
type Values map[string][]string
```

# **Example**

Below is the program for the same. This is how we created the body in the program

```
data := url.Values{}
data.Set("name", "John")
data.Set("age", "18")
data.Add("hobbies", "sports")
data.Add("hobbies", "music")
```

Difference between Add and Set

*   **Add** will append an extra value to the key in **url.Values** map

*   **Set** will replace the existing key in **url.Values** map

After that, we need to encode the data. Then encoded data is passed to the **http.NewRequest** function. Also don’t forget to set the content-type header to **application/x-www-form-urlencoded**

```
encodedData := data.Encode()
http.NewRequest(method, urlPath, strings.NewReader(encodedData))
```

The full program for the same.

```
package main

import (
	"fmt"
	"net/http"
	"net/url"
	"strconv"
	"strings"
	"time"
)

func main() {
	call("http://localhost:8080/employee", "POST")
}

func call(urlPath, method string) error {
	client := &http.Client{
		Timeout: time.Second * 10,
	}

	data := url.Values{}
	data.Set("name", "John")
	data.Set("age", "18")
	data.Add("hobbies", "sports")
	data.Add("hobbies", "music")
	encodedData := data.Encode()
	fmt.Println(encodedData)
	req, err := http.NewRequest(method, urlPath, strings.NewReader(encodedData))
	if err != nil {
		return fmt.Errorf("Got error %s", err.Error())
	}
	req.Header.Add("Content-Type", "application/x-www-form-urlencoded")
	req.Header.Add("Content-Length", strconv.Itoa(len(data.Encode())))
	response, err := client.Do(req)
	if err != nil {
		return fmt.Errorf("Got error %s", err.Error())
	}
	defer response.Body.Close()
	return nil
}
```

If you run a server that listens on :8080 port similar to the below link, then you will see that the server will be able to correctly print the body of the incoming request

[https://golangbyexample.com/url-encoded-body-golang](https://golangbyexample.com/url-encoded-body-golang)/

Also the output of

```
fmt.Println(encodedData)
```

will be

```
age=18&hobbies=sports&hobbies=music&name=John
```

which is in the same format that we had discussed above.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*