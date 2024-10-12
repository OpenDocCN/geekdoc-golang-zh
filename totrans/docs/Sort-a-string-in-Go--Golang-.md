<!--yml
category: 未分类
date: 2024-10-13 06:43:56
-->

# Sort a string in Go (Golang)

> 来源：[https://golangbyexample.com/sort-string-golang/](https://golangbyexample.com/sort-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")*  *## **Overview**

In Golang string is a sequence of bytes. A string literal actually represents a UTF-8 sequence of bytes. In UTF-8, ASCII characters are single-byte corresponding to the first 128 Unicode characters. All other characters are between 1 -4 bytes. To understand it more consider the below string

```
sample := "a£c"
```

In above string

*   ‘a’ takes one byte as per UTF-8

*   ‘£’ takes two bytes as per UTF-8

*   ‘b’ takes one byte as per UTF-8

The above string has 1+2+1 = 4 bytes altogether. Therefore when we try to print the length of the string using the standard **len()** function it will output 4 and not 3 as **len()** function returns the number of bytes in the string.

```
fmt.Printf("Length is %d\n", len(sample))
```

Therefore to sort a string we need to convert it to a rune array and then sort that using the **sort.Sort** function of go.

Below is the signature of **sort.Sort** function

```
func Sort(data Interface)
```

where this is the definition of **Interface**

```
type Interface interface {
	// Len is the number of elements in the collection.
	Len() int

	// Less reports whether the element with index i
	// must sort before the element with index j.
	Less(i, j int) bool

	// Swap swaps the elements with indexes i and j.
	Swap(i, j int)
}
```

So whatever we want to sort using the **sort.Sort** function then that needs to implement above three functions

*   Len() int

*   Less(i, j int) bool

*   Swap(i, j int)

In the below program, we did the same

```
package main

import (
	"fmt"
	"sort"
)

func main() {
	sortString("bac")
}

func sortString(input string) {
	runeArray := []rune(input)
	sort.Sort(sortRuneString(runeArray))
	fmt.Println(string(runeArray))
}

type sortRuneString []rune

func (s sortRuneString) Swap(i, j int) {
	s[i], s[j] = s[j], s[i]
}

func (s sortRuneString) Less(i, j int) bool {
	return s[i] < s[j]
}

func (s sortRuneString) Len() int {
	return len(s)
}
```

**Output**

```
abc
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*