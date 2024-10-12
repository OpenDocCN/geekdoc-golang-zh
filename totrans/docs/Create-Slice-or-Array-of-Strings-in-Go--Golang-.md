<!--yml
category: 未分类
date: 2024-10-13 06:40:16
-->

# Create Slice or Array of Strings in Go (Golang)

> 来源：[https://golangbyexample.com/slice-array-string-golang/](https://golangbyexample.com/slice-array-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Slice of String](#Slice_of_String "Slice of String")
*   [Array of Strings](#Array_of_Strings "Array of Strings")*  *## **Overview**

It is possible to create a slice or array of **string** data type in Golang as well. In fact, a slice or array can be created of any data type in Go. This tutorial contains simple examples to create a slice or array of **string** data type in golang.

Just to add here that in golang array is of fixed size and slice can have variable size. More details here

Array – [https://golangbyexample.com/understanding-array-golang-complete-guide/](https://golangbyexample.com/understanding-array-golang-complete-guide/)

Slice – [https://golangbyexample.com/slice-in-golang/](https://golangbyexample.com/slice-in-golang/)

## **Slice of String**

```
package main

import "fmt"

func main() {

	//First Way
	var string_first []string
	string_first = append(string_first, "abc")
	string_first = append(string_first, "def")
	string_first = append(string_first, "ghi")

	fmt.Println("Output for First slice of string")
	for _, c := range string_first {
		fmt.Println(c)
	}

	//Second Way
	string_second := make([]string, 3)
	string_second[0] = "ghi"
	string_second[1] = "def"
	string_second[2] = "abc"

	fmt.Println("\nOutput for Second slice of string")
	for _, c := range string_second {
		fmt.Println(c)
	}
}
```

**Output**

```
Output for First slice of string
abc
def
ghi

Output for Second slice of string
ghi
def
abc
```

We have two ways of creating a slice of strings. The first way is

```
var string_first []string
string_first = append(string_first, "abc")
string_first = append(string_first, "def")
string_first = append(string_first, "ghi")
```

In the second way, we use make command to create a slice of strings

```
string_second := make([]string, 3)
string_second[0] = "ghi"
string_second[1] = "def"
string_second[2] = "abc"
```

Either way works. This is how we can create a slice of strings

## **Array of Strings**

```
package main

import "fmt"

func main() {

	var string_first [3]string

	string_first[0] = "abc"
	string_first[1] = "def"
	string_first[2] = "ghi"

	fmt.Println("Output for First Array of string")
	for _, c := range string_first {
		fmt.Println(c)
	}

	string_second := [3]string{
		"ghi",
		"def",
		"abc",
	}

	fmt.Println("\nOutput for Second Array of string")
	for _, c := range string_second {
		fmt.Println(c)
	}
}
```

**Output**

```
Output for First Array of string
abc
def
ghi

Output for Second Array of string
ghi
def
abc
```

We have two ways of creating an array. The first way is

```
var string_first [3]string
string_first[0] = "abc"
string_first[1] = "def"
string_first[2] = "ghi"
```

In second way we directly initialize the array with some strings

```
string_second := [3]string{
	"ghi",
	"def",
	"abc",
}
```

Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*