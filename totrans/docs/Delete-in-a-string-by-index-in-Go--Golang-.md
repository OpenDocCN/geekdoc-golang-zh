<!--yml
category: 未分类
date: 2024-10-13 06:13:55
-->

# Delete in a string by index in Go (Golang)

> 来源：[https://golangbyexample.com/golang-delete-index-string/](https://golangbyexample.com/golang-delete-index-string/)

In Golang string is a sequence of bytes. A string literal actually represents a UTF-8 sequence of bytes. In UTF-8, ASCII characters are single-byte corresponding to the first 128 Unicode characters. All other characters are between 1 -4 bytes. Due to this, it is not possible to index a character in a string.  In GO, rune data type represents a Unicode point.  Once a string is converted to an array of rune then it is possible to index a character in that array of rune.

You can learn more about rune here – [https://golangbyexample.com/understanding-rune-in-golang](https://golangbyexample.com/understanding-rune-in-golang)

For this reason in below program for deleting a character of a given string by index, we  are first converting a string into a rune array so that we can index the rune array and then delete the character by index

```
package main

import "fmt"

func main() {
    sample := "ab£c"
    s := []rune(sample)
    res := delChar(s, 2)
    fmt.Println(string(res))
}

func delChar(s []rune, index int) []rune {
    return append(s[0:index], s[index+1:]...)
}
```

**Output:**

```
abc
```