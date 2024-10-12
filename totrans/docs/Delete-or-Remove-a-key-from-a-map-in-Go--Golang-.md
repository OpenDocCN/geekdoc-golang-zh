<!--yml
category: 未分类
date: 2024-10-13 06:52:35
-->

# Delete or Remove a key from a map in Go (Golang)

> 来源：[https://golangbyexample.com/delete-key-map-golang/](https://golangbyexample.com/delete-key-map-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Key exists in the Map](#Key_exists_in_the_Map "Key exists in the Map")
*   [The Key doesn’t exist in the Map](#The_Key_doesnt_exist_in_the_Map "The Key doesn’t exist in the Map")*  *# **Overview**

Below is the format to delete a given key from a map

```
delete(mapName, keyToDelete)
```

There are two cases

*   The **keyToDelete** exists in the map. In this case, it will simply delete the key

*   The **keyToDelete** doesn’t exist in the map. In this case, it will do nothing. A

Let’s see an example of both the cases

# **Key exists in the Map**

Below is the program for the case where the key exists in the Map

```
package main

import "fmt"

func main() {
	sample := make(map[string]int)
	sample["a"] = 1
	sample["b"] = 2
	sample["c"] = 3

        fmt.Println(sample)
	delete(sample, "a")

	fmt.Println(sample)
}
```

**Output**

```
map[a:1 b:2 c:3]
map[b:2 c:3]
```

Notice in the output that the key “a” got deleted from the map. It is always a good practice to check if a key exists in the map and then delete it. Below is the code snippet for that.

```
package main

import "fmt"

func main() {
	sample := make(map[string]int)
	sample["a"] = 1
	sample["b"] = 2
	sample["c"] = 3

	fmt.Println(sample)

	if _, ok := sample["a"]; ok {
		delete(sample, "a")
	}

	fmt.Println(sample)
}
```

**Output**

```
map[a:1 b:2 c:3]
map[b:2 c:3]
```

# **The Key doesn’t exist in the Map**

In this case, also it is a good practice to check if the key exists and then delete it. Even if we delete directly without checking, then also it is not a problem though. The below code snippet shows both the cases

```
package main

import "fmt"

func main() {
	sample := make(map[string]int)
	sample["a"] = 1
	sample["b"] = 2
	sample["c"] = 3

	fmt.Println(sample)

	//Check and delete
	if _, ok := sample["d"]; ok {
		delete(sample, "d")
	}

	fmt.Println(sample)

	//Directly delete
	delete(sample, "d")

	fmt.Println(sample)
}
```

**Output**

```
map[a:1 b:2 c:3]
map[a:1 b:2 c:3]
map[a:1 b:2 c:3]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –

[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)*