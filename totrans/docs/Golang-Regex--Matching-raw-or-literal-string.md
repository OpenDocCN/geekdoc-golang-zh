<!--yml
category: 未分类
date: 2024-10-13 06:37:18
-->

# Golang Regex: Matching raw or literal string

> 来源：[https://golangbyexample.com/golang-regex-literal-strin/](https://golangbyexample.com/golang-regex-literal-strin/)

Table of Contents

 **   [Overview](#Overview "Overview")
    *   [MatchCompile Function](#MatchCompile_Function "MatchCompile Function")
    *   [Match Method](#Match_Method "Match Method")*  *## **Overview**

We will be using **regexp** package in golang in our example that provides regular expression searching capabilities[https://golang.org/pkg/regexp/](https://golang.org/pkg/regexp/)

Before looking into the regex itself, let’s look at some basic functions or methods provided by Go to do a regex match.

### **MatchCompile Function**

[https://golang.org/pkg/regexp/#MustCompile](https://golang.org/pkg/regexp/#MustCompile)

Below is the signature of the function

```
func MustCompile(str string) *Regexp
```

We first compile the given regex string using the **MustCompile** function. This function panics if the given regex is not valid. After it is able to successfully compile the given regex, it returns the instance of regexp struct.

```
sampleRegexp := regexp.MustCompile("some_regular_expression"")
```

### **Match Method**

[https://golang.org/pkg/regexp/#Regexp.Match](https://golang.org/pkg/regexp/#Regexp.Match)

Below is the signature of the method

```
func (re *Regexp) Match(b []byte) bool
```

We can call the **Match** method on the regexp instance to match the given pattern with the regex. It returns true if the regex matches with the input string otherwise false. We need to pass in bytes of the input string to this method.

```
match := sampleRegexp.Match([]byte("some_string"))
```

Let’s see a simple program for a basic regex having a literal or raw string

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegex := regexp.MustCompile("abc")

	match := sampleRegex.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)

	match = sampleRegex.Match([]byte("1abc2"))
	fmt.Printf("For 1abc2: %t\n", match)

	match = sampleRegex.Match([]byte("xyz"))
	fmt.Printf("For xyz: %t\n", match)
}
```

**Output**

```
For abc: true
For 1abc2: true
For xyz: false
```

In the above program, we have a simple literal string regular expression

We first call **MustCompile** to check if the given regular expression is valid. After that, we match it with the below sample string or text

*   **String “abc”** – It gives a match and prints true

*   **String “1abc2”** – It gives a match and prints true. It matches as it contains **“abc”** as a substring.

*   **String “xyz”** – It does not give a match and prints false

As we can notice in the above program that if the given string or text contains the regex as a substring then also it gives a match. If we want to do full string matches then we need to use the anchor characters at the start and end of the regex.  **Caret** anchor character will be used at the start and **Dollar** anchor character will be used at the end.

Please refer to this article for details around full string matches

[https://golangbyexample.com/golang-regex-match-full-string](https://golangbyexample.com/golang-regex-match-full-string/)

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)*