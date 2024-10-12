<!--yml
category: 未分类
date: 2024-10-13 06:13:19
-->

# All permutations of a string in Golang

> 来源：[https://golangbyexample.com/all-permutations-string-golang/](https://golangbyexample.com/all-permutations-string-golang/)

In Golang string is a sequence of bytes. A string literal actually represents a UTF-8 sequence of bytes. In UTF-8, ASCII characters are single-byte corresponding to the first 128 Unicode characters. All other characters are between 1 -4 bytes. Due to this, it is not possible to index a character in a string.  In GO, rune data type represents a Unicode point.  Once a string is converted to an array of rune then it is possible to index a character in that array of **rune**.

You can learn more about rune here – [https://golangbyexample.com/understanding-rune-in-golang](https://golangbyexample.com/understanding-rune-in-golang)

For this reason in below program for generating permutations we are first converting a string into a rune array so that we can index the rune array to get the individual characters.

```
package main

import "fmt"

func main() {
    sample := "ab£"
    sampleRune := []rune(sample)
    generatePermutation(sampleRune, 0, len(sampleRune)-1)
}

func generatePermutation(sampleRune []rune, left, right int) {
    if left == right {
        fmt.Println(string(sampleRune))
    } else {
        for i := left; i <= right; i++ {
            sampleRune[left], sampleRune[i] = sampleRune[i], sampleRune[left]
            generatePermutation(sampleRune, left+1, right)
            sampleRune[left], sampleRune[i] = sampleRune[i], sampleRune[left]
        }
    }
}
```

**Output:**

```
ab£
a£b
ba£
b£a
£ba
£ab
```