<!--yml
category: 未分类
date: 2024-10-13 06:16:15
-->

# Pick a random character in string in Go (Golang)

> 来源：[https://golangbyexample.com/pick-random-character-string-golang/](https://golangbyexample.com/pick-random-character-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

**‘mat/rand’** package of golang contains a **Intn** function that can be used to generate a pseudo-random number between [0,n). Bracket at the end means that n is exclusive. This function can be utilized to pick a random element in a string. We can generate a random between 0 and length-1 of string. Then we can use that random number to index into the string and get the result.

But there is one problem in above approach. In Golang string is a sequence of bytes. A string literal actually represents a UTF-8 sequence of bytes. In UTF-8, ASCII characters are single-byte corresponding to the first 128 Unicode characters. All other characters are between 1 -4 bytes. Due to this it is not possible to index a character in a string.  In GO, rune data type represents a Unicode point.  Once a string is converted to an array of rune then it is possible to index a character in that array of rune.

You can learn more the above issue here – [https://golangbyexample.com/number-characters-string-golang/](https://golangbyexample.com/number-characters-string-golang/)

For this reason in below program for picking a random in a given string , we  are first converting a string into a rune array so that we can index into the rune array and then return the random character.

# **Code**

```
package main

import (
    "fmt"
    "math/rand"
)

func main() {
    in := "abcdedf£"
    inRune := []rune(in)
    randomIndex := rand.Intn(len(inRune))
    pick := inRune[randomIndex]
    fmt.Println(string(pick))
}
```

**Output:**

```
One of a,b,c,d,e,f,£
```

*   [character](https://golangbyexample.com/tag/character/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [pick](https://golangbyexample.com/tag/pick/)*   [random](https://golangbyexample.com/tag/random/)*   [string](https://golangbyexample.com/tag/string/)*