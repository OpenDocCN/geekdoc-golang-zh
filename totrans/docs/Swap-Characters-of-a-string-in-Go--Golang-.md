<!--yml
category: 未分类
date: 2024-10-13 06:13:24
-->

# Swap Characters of a string in Go (Golang)

> 来源：[https://golangbyexample.com/swap-characters-string-golang/](https://golangbyexample.com/swap-characters-string-golang/)

In Golang string is a sequence of bytes. A string literal actually represents a UTF-8 sequence of bytes. In UTF-8, ASCII characters are single-byte corresponding to the first 128 Unicode characters. All other characters are between 1 -4 bytes. Due to this, it is not possible to index a character in a string.  In GO, rune data type represents a Unicode point.  Once a string is converted to an array of rune then it is possible to index a character in that array of rune. You can learn more about rune here – [https://golangbyexample.com/understanding-rune-in-golang](https://golangbyexample.com/understanding-rune-in-golang)

For this reason in the below program for swapping characters in a string, we are first converting a string into a rune array so that we can index the rune array to get the individual characters.

```
package main

import "fmt"
func main() {
    sample := "ab£d"
    r := []rune(sample)

    fmt.Printf("Before %s\n", string(r))
    r[2], r[3] = r[3], r[2]

    fmt.Printf("After %s\n", string(r))
}
```

**Output:**

```
Before: ab£d
After: abd£
```