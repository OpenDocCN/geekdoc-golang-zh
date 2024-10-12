<!--yml
category: 未分类
date: 2024-10-13 06:16:30
-->

# Generate a random character in Go (Golang)

> 来源：[https://golangbyexample.com/generate-random-character-golang/](https://golangbyexample.com/generate-random-character-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# Overview

**‘mat/rand’** package of golang contains an **Intn** function that can be used to generate a random number between [0,n). The bracket at the end means that n is exclusive.

To know more about what pseudo-random number means, check out this post – [https://golangbyexample.com/generate-random-number-golang](https://golangbyexample.com/generate-random-number-golang)

Below is the signature of this method. It takes input a number n and will return a number x in range 0<=x<n.

```
func Intn(n int) int
```

The above function can also be used to generate a random character too. See below program, it is used to generate a character. We are also providing a seed value to the rand so that it generates different output. It is used to generate:

*   Random character between lowercase a to z

*   Random character between uppercase A and Z

*   Random character between uppercase A and Z  and lowercase a to z

# **Code**

```
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    rand.Seed(time.Now().Unix())

    //Generate a random character between lowercase a to z
    randomChar := 'a' + rune(rand.Intn(26))
    fmt.Println(string(randomChar))

    //Generate a random character between uppercase A and Z
    randomChar = 'A' + rune(rand.Intn(26))
    fmt.Println(string(randomChar))

    //Generate a random character between uppercase A and Z  and lowercase a to z
    randomInt := rand.Intn(2)
    if randomInt == 1 {
        randomChar = 'A' + rune(rand.Intn(26))
    } else {
        randomChar = 'a' + rune(rand.Intn(26))
    }
    fmt.Println(string(randomChar))
}
```

**Output:**

```
Will be lowercase between a to z
Will be uppercase between A to Z
Will be lowercase between a to z or uppsercase between A to Z
```

*   [character](https://golangbyexample.com/tag/character/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [random](https://golangbyexample.com/tag/random/)*