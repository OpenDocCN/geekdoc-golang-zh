<!--yml
category: 未分类
date: 2024-10-13 06:40:42
-->

# Get or Extract Query Params from a URL in Go (Golang)

> 来源：[https://golangbyexample.com/query-params-url-golang/](https://golangbyexample.com/query-params-url-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

Query function of the URL instance can be used to get the query params present in a URL

[https://pkg.go.dev/net/url#URL.Query](https://pkg.go.dev/net/url#URL.Query)

Note that query param is represented as below in Golang

[https://pkg.go.dev/net/url#Values](https://pkg.go.dev/net/url#Values)

which is

```
map[string][]string
```

which actually means a particular query param key value can have one or multiple values.

Example

```
http://localhost:8080/products?filters=color&filters=price&order=asc
```

In this URL **filters** key has two values – **color** and **price** while **order** key has only one value **asc**

# **Program**

Let’s see a working example

```
package main

import (
	"fmt"
	"log"
	"net/url"
)

func main() {
	input_url := "http://localhost:8080/products?filters=color&filters=price&order=asc"
	u, err := url.Parse(input_url)
	if err != nil {
		log.Fatal(err)
	}

	queryParams := u.Query()

	fmt.Println(queryParams)

	fmt.Println(queryParams["filters"])

	fmt.Println(queryParams["order"])

}
```

**Output**

```
map[filters:[color price] order:[asc]]
[color price]
[asc]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*