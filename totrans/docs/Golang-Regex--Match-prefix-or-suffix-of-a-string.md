<!--yml
category: 未分类
date: 2024-10-13 06:36:57
-->

# Golang Regex: Match prefix or suffix of a string

> 来源：[https://golangbyexample.com/regex-prefix-suffix-golang/](https://golangbyexample.com/regex-prefix-suffix-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Prefix Matching](#Prefix_Matching "Prefix Matching")
*   [Suffix Matching](#Suffix_Matching "Suffix Matching")*  *## **Overview**

Golang regex contains two anchor characters that can be used to match the prefix and suffix of a string given a regular expression. These two characters are

*   **Caret Character ‘^’ –** It is an anchor character that can be used to match the prefix of a string.

*   **Dollar Character ‘$’ –** It is an anchor character that can be used to match the suffix of a string.

Let’s look at prefix and suffix matching one by one

## **Prefix Matching**

It is an anchor character that can be used to match the prefix of a string.  It is used at the start of the regex to make sure that the given input string is matched with the regex from the start.  Basically, matches the beginning of the input string

```
package main

import (
    "fmt"
    "regexp"
)

func main() {
    sampleRegex := regexp.MustCompile("^abc")

    match := sampleRegex.Match([]byte("abcd"))
    fmt.Printf("For abcd: %t\n", match)

    match = sampleRegex.Match([]byte("abc"))
    fmt.Printf("For abc: %t\n", match)

    match = sampleRegex.Match([]byte("1abc23"))
    fmt.Printf("For 1abc23: %t\n", match)

    match = sampleRegex.Match([]byte("ab"))
    fmt.Printf("For ab: %t\n", match)
}
```

**Output**

```
For abcd: true
For abc: true
For 1abc23: false
For ab: false
```

In the above example, we used below regex

```
^abc
```

Notice the caret character at the start. It will match any string which starts with **“abc”**. The trailing character doesn’t matter.

It gives matches for all the string which start with **“abc”.** That is why it gives a match for

```
For abcd: true
For abc: true
```

and does not give a match for

```
For 1abc23: false
For ab: false
```

## **Suffix Matching**

It is an anchor character that can be used to match the suffix of a string. It is used at the end of the regex to make sure that the given input string is matched with the regex at the end. It matches the end of the input string.

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegex := regexp.MustCompile("abc$")

	match := sampleRegex.Match([]byte("1abc"))
	fmt.Printf("For 1abc: %t\n", match)

	match = sampleRegex.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)

	match = sampleRegex.Match([]byte("abcd"))
	fmt.Printf("For abcd: %t\n", match)

	match = sampleRegex.Match([]byte("ab"))
	fmt.Printf("For ab: %t\n", match)
}
```

**Output**

```
For 1abc: true
For abc: true
For abcd: false
For ab: false
```

In the above example, we used below regex

```
abc$
```

Notice the dollar character at the end. It will match any string which ends with **“abc”**. The starting characters doesn’t matter.

It gives matches for all the string which end with **“abc”.** That is why it gives a match for

```
For 1abc: true
For abc: true
```

and does not give a match for

```
For abcd: false
For ab: false
```

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

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*