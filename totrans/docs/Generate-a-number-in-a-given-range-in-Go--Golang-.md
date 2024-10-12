<!--yml
category: 未分类
date: 2024-10-13 06:16:40
-->

# Generate a number in a given range in Go (Golang)

> 来源：[https://golangbyexample.com/random-number-range-golang/](https://golangbyexample.com/random-number-range-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

**‘mat/rand’** package of golang contains a **Intn** function that can be used to generate a random number between [0,n). Bracket at the end means that n is exclusive.

To know more about what pseudo-random number means, checkout this post – [https://golangbyexample.com/generate-random-number-golang](https://golangbyexample.com/generate-random-number-golang)

Below is the signature of this method. It takes input a number n and will return a number x in range 0<=x<n.

```
func Intn(n int) int
```

Above function can also be used to generate a random number in range a to b too. See below program. It is used to generate a number between range a to b. We are also providing a seed value to the **rand** so that it generates different output everytime.

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

    //Generate a random number x where x is in range 5<=x<=20
    rangeLower := 5
    rangeUpper := 20
    randomNum := rangeLower + rand.Intn(rangeUpper-rangeLower+1)
    fmt.Println(randomNum)

    //Generate a random number x where x is in range 100<=x<=200
    rangeLower = 100
    rangeUpper = 200
    randomNum = rangeLower + rand.Intn(rangeUpper-rangeLower+1)
    fmt.Println(randomNum)
}
```

**Output:**

```
Number between 5<=x<=20
Number between 100<=x<=200
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [random](https://golangbyexample.com/tag/random/)*