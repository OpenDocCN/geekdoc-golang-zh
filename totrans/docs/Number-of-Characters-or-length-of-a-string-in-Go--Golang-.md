<!--yml
category: 未分类
date: 2024-10-13 06:14:15
-->

# Number of Characters or length of a string in Go (Golang)

> 来源：[https://golangbyexample.com/number-characters-string-golang/](https://golangbyexample.com/number-characters-string-golang/)

In golang string is a sequence of bytes. A string literal actually represents a UTF-8 sequence of bytes. In UTF-8, ASCII characters are single-byte corresponding to the first 128 Unicode characters. All other characters are between 1 -4 bytes. That is why it is not possible to get the exact length of the string using the built-in **len** function of go. It might work if the string only contains ASCII characters. But if the string contains Non-ASCII characters then it will give the correct output.

 For example, see below the program and its output. **£** in the string below is a Non-ASCII character.

```
package main

import "fmt"

func main() {
    sample := "ab£c"
    for i := 0; i < 4; i++ {
        fmt.Printf("%c\n", sample[i])
    }
    fmt.Printf("Length is %d\n", len(sample))
}
```

**Output:**

```
a
b
Â
£
Length is 5
```

As you might have noticed , it prints different characters than expected and length is also 5 instead of 4\. Why is that? To answer please remember we said that a string is essentially a slice of bytes. Let's print that slice of bytes using

```
sample := "ab£c"
fmt.Println([]byte(sample))
```

Output will be

```
[97 98 194 163 99]
```

This is the mapping of each of character to its byte sequence. As you can notice **a**, **b**, **c** take each 1 byte but **£** takes two bytes. That is why the length of the string is 5 and not 4

| a | 97 |
| b | 98 |
| £ | 194, 163 |
| c | 99 |

There are two alternatives to get the correct length of a string

*   Using rune data type . **rune** data type represents a Unicode point.  Once a string is converted to an array of **rune** then it is possible to get the correct length of a string.

*   Using range operator over the string and calculating the length. range operator iterates over the UTF-8 characters in a string.

The below code illustrates the above two points.

```
package main

import "fmt"

func main() {
    sample := "ab£c"
    //Using rune
    s := []rune(sample)
    fmt.Println(len(s))

    //Using range
    len := 0
    for range sample {
        len++
    }
    fmt.Println(len)
}
```

**Output:**

```
4
4
```