<!--yml
category: 未分类
date: 2024-10-13 06:35:17
-->

# Golang Regex – Include dot – ‘.’ inside square brackets or character class

> 来源：[https://golangbyexample.com/dot-square-bracket-regex-golang/](https://golangbyexample.com/dot-square-bracket-regex-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Dot or **‘.’** is treated as a literal character inside the square brackets or character class. It doesn’t need to be escaped inside that. Let’s see a working program for the same as well

We will be using **regexp** package in golang in our example that provides regular expression searching capabilities

[https://golang.org/pkg/regexp/](https://golang.org/pkg/regexp/)

## **Program**

```
package main

import (
    "fmt"
    "regexp"
)

func main() {
    sampleRegex := regexp.MustCompile("[.]")
    match := sampleRegex.Match([]byte("."))
    fmt.Println(match)
}
```

**Output**

```
true
```

This regex

```
[.]
```

will match a single dot or **‘.’**

We first compile the given regex using the MustCompile function. This function panics if the given regex is not valid. After it is able to successfully compile the given regex, it returns the instance of regexp struct.

```
sampleRegexp := regexp.MustCompile("[.]")
```

We can call the Match method on the regexp instance to match the given pattern with the regex. It returns true if the regex matches with the input string otherwise false. We need to pass in bytes of the input string to this method.

```
match := sampleRegexp.Match([]byte("."))
```

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*