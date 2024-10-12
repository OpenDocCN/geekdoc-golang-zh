<!--yml
category: 未分类
date: 2024-10-13 06:16:20
-->

# Shuffle a string in Go (Golang)

> 来源：[https://golangbyexample.com/shuffle-string-golang/](https://golangbyexample.com/shuffle-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**math/rand** package of GO provides a **Shuffle** method that can be used to shuffle a string. This method pseudo-randomizes the order of elements using the default source. pseudo-randomizes means that for a fixed input seed it will generate the same randomization. That is why in our program we will initialize the rand package with a different seed every time.

Below is the signature of the function.

```
func Shuffle(n int, swap func(i, j int))
```

This function takes in below arguments

*   First is the total number of characters in the given string.
*   The second is a swap function that will be called for different indexes **i** and **j.** You need to provide your own swap function that will swap your elements in the string.

Also note that this function will panic if n<0\. In Golang string is a sequence of bytes. A string literal actually represents a UTF-8 sequence of bytes. In UTF-8, ASCII characters are single-byte corresponding to the first 128 Unicode characters. All other characters are between 1 to 4 bytes. Due to this it is not possible to index a character in a string.  In GO, rune data type represents a Unicode point.  Once a string is converted to an array of rune then it is possible to index a character in that array of rune.

You can learn more the above issue here – [https://golangbyexample.com/number-characters-string-golang/](https://golangbyexample.com/number-characters-string-golang/)

For this reason in the below program for shuffling a given string , we are first converting a string into a rune array so that we can index into the rune array and use that index to swap characters in that string to shuffle the string.

# **Code:**

```
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    rand.Seed(time.Now().Unix())

    in := "abcdedf£"

    inRune := []rune(in)
    rand.Shuffle(len(inRune), func(i, j int) {
        inRune[i], inRune[j] = inRune[j], inRune[i]
    })
    fmt.Println(string(inRune))

    rand.Shuffle(len(inRune), func(i, j int) {
        inRune[i], inRune[j] = inRune[j], inRune[i]
    })
    fmt.Println(string(inRune))
}
```

**Output:**

It will produce a different output on your machine.

```
dd£cebaf
feb£cadd
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [shuffle](https://golangbyexample.com/tag/shuffle/)*   [string](https://golangbyexample.com/tag/string/)*