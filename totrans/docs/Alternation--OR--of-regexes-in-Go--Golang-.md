<!--yml
category: 未分类
date: 2024-10-13 06:36:41
-->

# Alternation (OR) of regexes in Go (Golang)

> 来源：[https://golangbyexample.com/alternation-regex-golang/](https://golangbyexample.com/alternation-regex-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

It is similar to OR operation. Join two regexes by using the | operator. If there are two regexes **r1** and **r2**, then alternation is represented as below

```
r1|r2
```

It will either match **r1** or **r2** with preference given to **rq**. Basically, if string **s** matches regex **r1** and string **t** matches regex **r2**, then **r1|r2** will match **s** or **t**.  What does it mean when we say that preference will be given to **r1**. It means that if in the given sample string it will try to match **r1** first and if **r1** is not found, it will try to match **r2**.

## **Program**

```
package main
import (
    "fmt"
    "regexp"
)
func main() {
    sampleRegex := regexp.MustCompile("abc|xyz")
    match := sampleRegex.Match([]byte("abc"))

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
true
true
false
```

It matches

```
abc
xyz
```

It also matches.

```
abcxyz
```

This is because it matches the prefix **“abc”** and gives a true match.

Also, it does not match

```
abd
```

Alternation can also be between more than 2 regexes. Below is an example for the same

```
package main
import (
    "fmt"
    "regexp"
)
func main() {
    sampleRegex := regexp.MustCompile("abc|xyz|123")

    match := sampleRegex.Match([]byte("abc"))
    fmt.Println(match)

    match = sampleRegex.Match([]byte("xyz"))
    fmt.Println(match)

    match = sampleRegex.Match([]byte("123"))
    fmt.Println(match)

    match = sampleRegex.Match([]byte("abcxyz123"))
    fmt.Println(match)

    match = sampleRegex.Match([]byte("abd"))
    fmt.Println(match)
}
```

**Output**

```
true
true
true
true
false
```

It matches

```
abc
xyz
123
```

It also matches

```
abcxyz123
```

This is because it matches the prefix **“abc”** and gives a true match.

Also, it does not match

```
abd
```

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*