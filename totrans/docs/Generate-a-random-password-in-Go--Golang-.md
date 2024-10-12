<!--yml
category: 未分类
date: 2024-10-13 06:16:04
-->

# Generate a random password in Go (Golang)

> 来源：[https://golangbyexample.com/generate-random-password-golang/](https://golangbyexample.com/generate-random-password-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

**‘math/rand’** package of golang contains an Intn function that can be used to generate a pseudo-random number between [0,n). The bracket at the end means that n is exclusive. This function can be utilized to generate a random password from a character set.

To know more about what pseudo-random number means, check out this post – [https://golangbyexample.com/generate-random-number-golang](https://golangbyexample.com/generate-random-number-golang)

Below is the signature of this method. It takes input a number n and will return a number x in range 0<=x<n.

```
func Intn(n int) int
```

In the code below we have first defined the character set from which to generate the password

```
lowerCharSet   = "abcdedfghijklmnopqrst"
upperCharSet   = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
specialCharSet = "!@#$%&*"
numberSet      = "0123456789"
allCharSet     = lowerCharSet + upperCharSet + specialCharSet + numberSet
```

The below code also handles the case where the password can have minimum requirement of a specific number of special character, numeric or uppercase character.

Also please note once the password is generated with required criteria we shuffle it at the end. For shuffling, we convert it to a rune array first. Please refer to this article to understand why for shuffling a string – [https://golangbyexample.com/shuffle-string-golang/](https://golangbyexample.com/shuffle-string-golang/)

# **Code**

```
package main

import (
    "fmt"
    "math/rand"
    "strings"
    "time"
)

var (
    lowerCharSet   = "abcdedfghijklmnopqrst"
    upperCharSet   = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    specialCharSet = "!@#$%&*"
    numberSet      = "0123456789"
    allCharSet     = lowerCharSet + upperCharSet + specialCharSet + numberSet
)

func main() {
    rand.Seed(time.Now().Unix())
    minSpecialChar := 1
    minNum := 1
    minUpperCase := 1
    passwordLength := 8
    password := generatePassword(passwordLength, minSpecialChar, minNum, minUpperCase)
    fmt.Println(password)

    minSpecialChar = 2
    minNum = 2
    minUpperCase = 2
    passwordLength = 20
    password = generatePassword(passwordLength, minSpecialChar, minNum, minUpperCase)
    fmt.Println(password)
}

func generatePassword(passwordLength, minSpecialChar, minNum, minUpperCase int) string {
    var password strings.Builder

    //Set special character
    for i := 0; i < minSpecialChar; i++ {
        random := rand.Intn(len(specialCharSet))
        password.WriteString(string(specialCharSet[random]))
    }

    //Set numeric
    for i := 0; i < minNum; i++ {
        random := rand.Intn(len(numberSet))
        password.WriteString(string(numberSet[random]))
    }

    //Set uppercase
    for i := 0; i < minUpperCase; i++ {
        random := rand.Intn(len(upperCharSet))
        password.WriteString(string(upperCharSet[random]))
    }

    remainingLength := passwordLength - minSpecialChar - minNum - minUpperCase
    for i := 0; i < remainingLength; i++ {
        random := rand.Intn(len(allCharSet))
        password.WriteString(string(allCharSet[random]))
    }
    inRune := []rune(password.String())
	rand.Shuffle(len(inRune), func(i, j int) {
		inRune[i], inRune[j] = inRune[j], inRune[i]
	})
	return string(inRune)
}
```

**Output**

Output will be different on your machine

```
e$e&tD19
4T$&Qmg2rA4DXD5a2OGs
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [random password](https://golangbyexample.com/tag/random-password/)*