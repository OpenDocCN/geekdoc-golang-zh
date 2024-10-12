<!--yml
category: 未分类
date: 2024-10-13 06:36:52
-->

# Golang regex: Understanding caret and dollar character

> 来源：[https://golangbyexample.com/golang-regex-understanding-caret-and-dollar-character/](https://golangbyexample.com/golang-regex-understanding-caret-and-dollar-character/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

**Caret character****‘^’** and **Dollar character****‘$’** are metacharacters that are used in regex in golang. Metacharacters are characters that have special meaning in regex. Below is a brief introduction of the caret character and dollar character in golang.

*   **Caret Character ‘^’ –** It is an anchor character that is used at the start of the regex to make sure that the given input string is matched with the regex from the start.  It matches the beginning of the input string

*   **Dollar Character ‘$’ –** It is also an anchor character that is used at the end of the regex to make sure that the given input string is matched with the regex at the end.  It matches the end of the input string

## **Program**

Caret and Dollar Metacharacters are anchor characters that are used to match the start and end of the string respectively. Not clear? Let’s understand it with an example. Let’s say you have below regex having three simple characters

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

The above program gives a match for

```
abc
```

But doesn’t give a match for below as it only matches the full string

```
abcd
1abc23
```

This was the case when we wanted to match full string. What if we also want to match the string which starts with **“abc”**. The trailing character doesn’t matter. How do we do it? You guessed it right. In this case, we will only use the **caret** character at the start of regex and not use the **dollar** character. See below example

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

If we want to match literal **caret** ^ then we need to escape it with a backslash. See below example

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegex := regexp.MustCompile("\\^abc")

	match := sampleRegex.Match([]byte("^abc"))
	fmt.Printf("For ^abc: %t\n", match)

	match = sampleRegex.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)
}
```

**Output**

```
For abcd: true
For abcd: false
```

What if we also want to match the string which ends with **“abc”**. The starting character doesn’t matter. How do we do it?  In this case, we will only use the **dollar** character at the start of regex and not use the **caret** character at the start. See below example

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

If we want to match literal dollar $ then we need to escape it with a backslash. See below example

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegex := regexp.MustCompile("abc\\$")

	match := sampleRegex.Match([]byte("abc$"))
	fmt.Printf("For abc$: %t\n", match)

	match = sampleRegex.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)
}
```

**Output**

```
For abc$: true
For abc: false
```

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)*