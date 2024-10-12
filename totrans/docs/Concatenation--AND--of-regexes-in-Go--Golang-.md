<!--yml
category: 未分类
date: 2024-10-13 06:36:36
-->

# Concatenation (AND) of regexes in Go (Golang)

> 来源：[https://golangbyexample.com/concatenation-regex-golang/](https://golangbyexample.com/concatenation-regex-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

It is similar to **AND** operator. For example, if there are two regexes **r1** and **r2**, the concatenation of regexes will be represented as below.

```
r1r2
```

It will match regex **r1** followed by **r2**. Both **r1** and **r2** should be valid regexes. Basically, if string **s** matches regex **r1** and string **t** matches regex **r2**, then string **st** will match **r1r2**.

## **Program**

Here is the example program for the same

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	first := "abc"
	second := "xyz"
	sampleRegex := regexp.MustCompile(first + second)

	match := sampleRegex.Match([]byte("abcxyz"))
	fmt.Println(match)

	match = sampleRegex.Match([]byte("abc"))
	fmt.Println(match)

	match = sampleRegex.Match([]byte("xyz"))
	fmt.Println(match)

	match = sampleRegex.Match([]byte("abd"))
	fmt.Println(match)
}
```

**Output**

```
true
false
false
false
```

It only matches

```
abcxyz
```

We first compile the given regex using the **[MustCompile](https://golang.org/pkg/regexp/#MustCompile)** function. This function panics if the given regex is not valid. After it is able to successfully compile the given regex, it returns the instance of **[regexp](https://golang.org/pkg/regexp/)** struct.

```
sampleRegexp := regexp.MustCompile(first + second)
```

We can call the **[Match](https://golang.org/pkg/regexp/#Match)** method on the regexp instance to match the given pattern with the regex. It returns true if the regex matches with the input string otherwise false. We need to pass in bytes of the input string to this method.

```
match := sampleRegexp.Match([]byte("abcxyz"))
```

Concatenation can also be done between more than three regexes. Below is an example for the same

```
package main

import (
    "fmt"
    "regexp"
)
func main() {
    first := "abc"
    second := "xyz"
    third := "123"
    sampleRegex := regexp.MustCompile(first + second + third)

    match := sampleRegex.Match([]byte("abcxyz123"))
    fmt.Println(match)

    match = sampleRegex.Match([]byte("abc"))
    fmt.Println(match)

    match = sampleRegex.Match([]byte("xyz"))
    fmt.Println(match)

    match = sampleRegex.Match([]byte("abcxyz"))
    fmt.Println(match)

    match = sampleRegex.Match([]byte("abd"))
    fmt.Println(match)
}
```

**Output**

```
true
false
false
false
false
```

It only matches

```
abcxyz123
```

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)*