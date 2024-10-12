<!--yml
category: 未分类
date: 2024-10-13 06:13:32
-->

# Reverse a string in Go (Golang)

> 来源：[https://golangbyexample.com/reverse-a-string-in-golang/](https://golangbyexample.com/reverse-a-string-in-golang/)

In Golang string is a sequence of bytes. A string literal actually represents a UTF-8 sequence of bytes. In UTF-8, ASCII characters are single-byte corresponding to the first 128 Unicode characters. All other characters are between 1 -4 bytes. Due to this, it is not possible to index a character in a string.  In GO, rune data type represents a Unicode point.  Once a string is converted to an array of rune then it is possible to index a character in that array of **rune**. You can learn more about rune here – [https://golangbyexample.com/understanding-rune-in-golang](https://golangbyexample.com/understanding-rune-in-golang)

For this reason in the below program for reverse a string,  we are first converting a string into a rune array so that we can index the rune array to get the individual characters. Once we have the individual characters, we can keep appending it to a new string starting from the end to start.

```
package main

import "fmt"

func main() {
    sample := "ab£d"
    r := []rune(sample)
    var res []rune
    for i := len(r) - 1; i >= 0; i-- {
        res = append(res, r[i])
    }
    fmt.Printf("Result: %s\n", string(res))
}
```

**Output:**

```
Result: d£ba
```