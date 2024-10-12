<!--yml
category: 未分类
date: 2024-10-13 06:36:47
-->

# Golang regex: Match full string

> 来源：[https://golangbyexample.com/golang-regex-match-full-string/](https://golangbyexample.com/golang-regex-match-full-string/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Golang regex contains two anchor characters that can be used to match a full string. These two characters are

*   **Caret Character ‘^’ –** It is an anchor character that is used at the start of the regex to make sure that the given input string is matched with the regex from the start.  It matches the beginning of the input string

*   **Dollar Character ‘$’ –** It is also an anchor character that is used at the end of the regex to make sure that the given input string is matched with the regex at the end.  It matches the end of the input string

Caret and Dollar Metacharacters are anchor characters that are used to match the start and end of the string respectively. Let’s first understand what happens if we don’t use these anchor characters. Let’s say we have below regex having three simple characters.

```
abc
```

This regex will match any string having **abc** as a substring. See below example

```
package main

import (
    "fmt"
    "regexp"
)

func main() {
    sampleRegex := regexp.MustCompile("abc")
    match := sampleRegex.Match([]byte("abcd"))
    fmt.Printf("For abcd: %t\n", match)

    match = sampleRegex.Match([]byte("1abc23"))
    fmt.Printf("For 1abc23: %t\n", match)

    match = sampleRegex.Match([]byte("abc"))
    fmt.Printf("For abc: %t\n", match)
}
```

**Output**

```
For abcd: true
For 1abc23: true
For abc: true
```

The above program gives a match for all the strings which contain **“abc”** as a substring.  Therefore it gives matches for

```
abc
abcd
1abc23
```

Basically, it will give a match for any input string which contains **“abc”** as a substring.

## **Program**

If we only want to match the complete string then we need to anchor the string with the **caret** character at the start and the **dollar** character at the end. This will enable us to do a full-string match. See the below example.

```
package main
import (
    "fmt"
    "regexp"
)
func main() {
    sampleRegex := regexp.MustCompile("^abc$")
    match := sampleRegex.Match([]byte("abcd"))

    fmt.Printf("For abcd: %t\n", match)
    match = sampleRegex.Match([]byte("1abc23"))
    fmt.Printf("For 1abc23: %t\n", match)

    match = sampleRegex.Match([]byte("abc"))
    fmt.Printf("For abc: %t\n", match)
}
```

**Output**

```
For abcd: false
For 1abc23: false
For abc: true
```

In the above program, the regex is

```
^abc$
```

The above program gives a match for

```
abc
```

But doesn’t give a match for below as it only matches the full string

```
abcd
1abc23
```

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)*