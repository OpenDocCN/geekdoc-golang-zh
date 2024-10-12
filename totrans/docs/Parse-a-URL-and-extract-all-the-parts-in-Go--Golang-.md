<!--yml
category: 未分类
date: 2024-10-13 06:40:21
-->

# Parse a URL and extract all the parts in Go (Golang)

> 来源：[https://golangbyexample.com/parse-url-golang/](https://golangbyexample.com/parse-url-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

**net/url** package of golang contains a Parse function that can be used to parse a given and return the url instance of the URL struct

[https://golang.org/pkg/net/url/#URL](https://golang.org/pkg/net/url/#URL)

Once the given URL is parsed correctly, then it will return the URI object. We can then access the below information from the URI

*   Scheme

*   User Info

*   Hostname

*   Port

*   Pathname

*   Query Params

*   Fragment

Let’s see a working program for the same:

We will parse the below URL

```
https://test:abcd123@golangbyexample.com:8000/tutorials/intro?type=advance&compact=false#history
```

Then

*   Scheme is HTTPS

*   User Info – username is test and password is abcd123\. Username and password are separated by a colon :

*   Hostname is [www.golangbyexample.com](http://www.golangbyexample.com/)

*   Port is 8000

*   The path is tutorials/intro

*   Query Params is **type=advance** and **compact=false**. They are separated by ampersand

*   fragment is **history**. It will directly take to the history section within that page. **history** is an identifier that refers to that section within that page

# **Program**

```
package main

import (
	"fmt"
	"log"
	"net/url"
)

func main() {
	input_url := "https://test:abcd123@golangbyexample.com:8000/tutorials/intro?type=advance&compact=false#history"
	u, err := url.Parse(input_url)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println(u.Scheme)
	fmt.Println(u.User)
	fmt.Println(u.Hostname())
	fmt.Println(u.Port())
	fmt.Println(u.Path)
	fmt.Println(u.RawQuery)
	fmt.Println(u.Fragment)
	fmt.Println(u.String())
}
```

**Output**

```
https
test:abcd123
golangbyexample.com
8000
/tutorials/intro
type=advance&compact=false
history
https://test:abcd123@golangbyexample.com:8000/tutorials/intro?type=advance&compact=false#history
```

It correctly dumps all the information as seen from the output

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*