<!--yml
category: 未分类
date: 2024-10-13 06:40:37
-->

# Get full hostname along with port from a URL in Go (Golang)

> 来源：[https://golangbyexample.com/hostname-port-url-golang/](https://golangbyexample.com/hostname-port-url-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

**net/url** package of golang contains a Parse function that can be used to parse a given and return the URL instance of the URL struct[https://golang.org/pkg/net/url/#URL](https://golang.org/pkg/net/url/#URL)

Once the given URL is parsed correctly, then it will return the URI object. We can then access the below information from the URI

*   Scheme

*   User Info

*   Hostname

*   Port

*   Pathname

*   Query Params

*   Fragment

Once we have all the parts we can concatenate them to get the full hostname along with the port. We will parse the below URL

```
https://test:abcd123@golangbyexample.com:8000/tutorials/intro?type=advance&compact=false#history
```

Then the full hostname along with port will be

```
https://golangbyexample.com:8000
```

# **Program**

Below is the program for the same.

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

	fullHostname := u.Scheme + "://" + u.Host

	fmt.Println(fullHostname)
}
```

**Output**

```
https://golangbyexample.com:8000:8000
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*