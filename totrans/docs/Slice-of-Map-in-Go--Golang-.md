<!--yml
category: 未分类
date: 2024-10-13 06:39:51
-->

# Slice of Map in Go (Golang)

> 来源：[https://golangbyexample.com/slice-map-golang/](https://golangbyexample.com/slice-map-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

It is possible to create a slice of map data type in Golang as well. In fact, a slice can be created of any data type in Go. Below is a simple example to create a slice of map

## **Program**

```
package main

import "fmt"

func main() {
	maps := make([]map[string]string, 3)

	map1 := make(map[string]string)
	map1["1"] = "a"

	map2 := make(map[string]string)
	map2["2"] = "b"

	map3 := make(map[string]string)
	map3["3"] = "c"

	maps[0] = map1
	maps[1] = map2
	maps[2] = map3

	for _, m := range maps {
		fmt.Println(m)
	}
}
```

**Output**

```
map[1:a]
map[2:b]
map[3:c]
```

In the above program, we created three maps of type **map[string]string**

```
map1 := make(map[string]string)
map1["1"] = "a"

map2 := make(map[string]string)
map2["2"] = "b"

map3 := make(map[string]string)
map3["3"] = "c"
```

We also created a slice of map data type like this

```
maps := make([]map[string]string, 3)
```

This is how we can create a slice of map

Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*