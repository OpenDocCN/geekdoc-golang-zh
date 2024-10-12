<!--yml
category: 未分类
date: 2024-10-13 06:16:46
-->

# Generate a random string in Go (Golang)

> 来源：[https://golangbyexample.com/generate-random-string-golang/](https://golangbyexample.com/generate-random-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

**‘mat/rand’** package of golang contains a **Intn** function that can be used to generate a pseudo-random number between [0,n). Bracket at the end means that n is exclusive. This function can be utilized to generate a random string from a character set.

To know more about what pseudo-random number means, checkout this post – [https://golangbyexample.com/generate-random-number-golang](https://golangbyexample.com/generate-random-number-golang)

Below is the signature of this method. It takes input a number n and will return a number x in range 0<=x<n.

```
func Intn(n int) int
```

Above function can be used to generate a random string. Basically we first select a charSet. Then we use the above function to generate a random number and then use that random number to get a random character from the charSet. This random character is added to a string until we have a random string of desired length.

# **Code**

```
package main

import (
    "fmt"
    "math/rand"
    "strings"
    "time"
)

func main() {
    rand.Seed(time.Now().Unix())

    //Only lowercase
    charSet := "abcdedfghijklmnopqrst"
    var output strings.Builder
    length := 10
    for i := 0; i < length; i++ {
        random := rand.Intn(len(charSet))
        randomChar := charSet[random]
        output.WriteString(string(randomChar))
    }
    fmt.Println(output.String())
    output.Reset()

    //Lowercase and Uppercase Both
    charSet = "abcdedfghijklmnopqrstABCDEFGHIJKLMNOP"
    length = 20
    for i := 0; i < length; i++ {
        random := rand.Intn(len(charSet))
        randomChar := charSet[random]
        output.WriteString(string(randomChar))
    }
    fmt.Println(output.String())
}
```

**Output:**

Below is the output on my machine. On your's it might give a different output

```
himsemkpkd
nHaiEpccEdBfCFPtaBbi
```

In above program we are using character set as

```
abcdedfghijklmnopqrst and abcdedfghijklmnopqrstABCDEFGHIJKLMNOP
```

All the above characters in charSet were ASCII characters hence we were able to index a character in **charSet** string. But this could be problem if the **charSet** contains non-ASCII character.

In Golang string is a sequence of bytes. A string literal actually represents a UTF-8 sequence of bytes. In UTF-8, ASCII characters are single-byte corresponding to the first 128 Unicode characters. All other characters are between 1 to 4 bytes. Due to this it is not possible to index a character in a string.  In GO, rune data type represents a Unicode point.  Once a string is converted to an array of rune then it is possible to index a character in that array of rune.

So in case, the character set contains some characters that are not ASCII they might occupy more than 1 bytes. In that case, we cannot use the above code to generate a random string as we cannot index into the charSet. For this case, we have to first convert a string into a rune array so that we can index into the rune array to the character and then incrementally form the random string.

As in the below example, our charSet contains a non-ASCII character **'£'.** This character occupies two bytes

```
package main

import (
    "fmt"
    "math/rand"
    "strings"
    "time"
)

func main() {
    rand.Seed(time.Now().Unix())
    //Only lowercase and £
    charSet := []rune("abcdedfghijklmnopqrst£")
    var output strings.Builder
    length := 10
    for i := 0; i < length; i++ {
        random := rand.Intn(len(charSet))
        randomChar := charSet[random]
        output.WriteRune(randomChar)
    }
    fmt.Println(output.String())
    output.Reset()

   //Lowercase and Uppercase Both and £
    charSet = []rune("abcdedfghijklmnopqrstABCDEFGHIJKLMNOP£")
    length = 20
    for i := 0; i < length; i++ {
        random := rand.Intn(len(charSet))
        randomChar := charSet[random]
        output.WriteRune(randomChar)
    }
    fmt.Println(output.String())
}
```

**Output**:

Below is the output on my machine. On yours it might give a different output

```
aidqpbse£j
rebhjblsePsLpGBPOhfB
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [random](https://golangbyexample.com/tag/random/)*   [string](https://golangbyexample.com/tag/string/)*