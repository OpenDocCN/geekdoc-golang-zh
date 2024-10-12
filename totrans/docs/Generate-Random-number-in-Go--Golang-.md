<!--yml
category: 未分类
date: 2024-10-13 06:15:53
-->

# Generate Random number in Go (Golang)

> 来源：[https://golangbyexample.com/generate-random-number-golang/](https://golangbyexample.com/generate-random-number-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [What is a pseudo-random number](#What_is_a_pseudo-random_number "What is a pseudo-random number ")
*   [Random Generator Functions in rand package](#Random_Generator_Functions_in_rand_package "Random Generator Functions in rand package")
    *   [Pseudo-Random Number Generator Functions with range.](#Pseudo-Random_Number_Generator_Functions_with_range "Pseudo-Random Number Generator Functions with range. ")
    *   [Pseudo Random Number Generator Functions without range.](#Pseudo_Random_Number_Generator_Functions_without_range "Pseudo Random Number Generator Functions without range. ")
    *   [Pseudo Random Number Generator Functions with range for floats](#Pseudo_Random_Number_Generator_Functions_with_range_for_floats "Pseudo Random Number Generator Functions with range for floats")
*   [Code](#Code "Code")*  *# **Overview**

Go provide a **‘math/rand’** package which has inbuilt support for generating pseudo-random numbers. This package defines methods which can be used to generate

*   A pseudo-random number within the range from 0 to n
*   A pseudo-random number without range specified. The range will depend upon the type of int i.e int64, int32, uint64, etc

# **What is a pseudo-random number**

Before proceeding let’s first understand what pseudo-random number means. Pseudo-random number is not truly random as its value is completed determined by the initial value known as seed.

To understand the role of seed, let’s first look at the very basic function which can generate a random number in range [0, n). The below function in the **rand** package can be used to generate pseudo-random number in range [0, n). Bracket at the end in [0,n) means that n is exclusive.

```
func Intn(n int) int
```

The above function returns an int value between 0 to n. Let’s write a program without seed value. We have passed 10, so below function will generate random numbers in range [0,10)

```
package main

import (
    "fmt"
    "math/rand"
)

func main() {
    fmt.Println(rand.Intn(10))
    fmt.Println(rand.Intn(10))
    fmt.Println(rand.Intn(10))
}
```

Try running above the program multiple times. It will give the same output every time. On my system, it gives below output

```
7
7
7
```

Now let’s try running the same program but first providing seed value.

```
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    rand.Seed(time.Now().Unix())

    fmt.Println(rand.Intn(10))
    fmt.Println(rand.Intn(10))
    fmt.Println(rand.Intn(10))
```

We are giving seed value as number of seconds which has elapsed till January 1, 1970, UTC.

```
rand.Seed(time.Now().Unix())
```

It gives different output each time you execute this program as seed value is different. That is what is meant when we say that go generates pseudo-random numbers.

# **Random Generator Functions in rand package**

Now we have understood what pseudo-random number generation, let’s look at some of the function provided by the rand package for generation of random numbers. You can use any of these functions to generate a random number based on your requirements.

## **Pseudo-Random Number Generator Functions with range**.

All functions take n as an argument and will panic if n<=0.

*   Intn(n int) – It returns a non-negative pseudo-random number in [0,n)

*   Int31n(n int32) – It returns a non-negative pseudo-random number in [0,n) but returns a int32

*   Int63n(n int64) – It returns a non-negative pseudo-random number in [0,n) but returns a int64

## **Pseudo Random Number Generator Functions without range.**

*   Int() – returns a non-negative pseudo-random int

*   Int31() – returns a non-negative pseudo-random 31-bit integer as an int32

*   Int63() – returns a non-negative pseudo-random 63-bit integer as an int64

*   Uint32() – returns a pseudo-random 32-bit value as a uint32

*   Uint64() – returns a pseudo-random 64-bit value as a uint64

## **Pseudo Random Number Generator Functions with range for floats**

*   Float64() – returns, as a float64, a pseudo-random number in [0.0,1.0)

*   Float32() – returns, as a float32, a pseudo-random number in [0.0,1.0)

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
    //Pseudo Random Number Generator Functions with range

    //1\. Intn(n int)
    fmt.Printf("Intn: %d\n", rand.Intn(10))

    //2\. Int31n(n int32)
    fmt.Printf("Int31n: %d\n", rand.Int31n(10))

    //3\. Int64n(n int32)
    fmt.Printf("Int64n: %d\n", rand.Int63n(10))

    //Pseudo Random Number Generator Functions without range.

    //1\. Int()
    fmt.Printf("Int: %d\n", rand.Int())

    //2\. Int31()
    fmt.Printf("Int31: %d\n", rand.Int31())

    //3\. Int63()
    fmt.Printf("Int63: %d\n", rand.Int63())

    //4\. Uint32()
    fmt.Printf("Uint32: %d\n", rand.Uint32())

    //4\. Uint64()
    fmt.Printf("Uint64: %d\n", rand.Uint64())

    //Pseudo Random Number Generator Functions with range for floats

    //1\. Float64()
    fmt.Printf("Float64: %f\n", rand.Float64())

    //2\. Float32()
    fmt.Printf("Float32: %f\n", rand.Float32())
}
```

**Output:**

```
Intn: 9
Int31n: 4
Int64n: 4
Int: 6567086139449890598
Int31: 402632083
Int63: 428924242891364663
Uint32: 1991553101
Uint64: 825780166485441015
Float64: 0.683701
Float32: 0.382141
```

*   [generate](https://golangbyexample.com/tag/generate/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [numbers](https://golangbyexample.com/tag/numbers/)*   [random](https://golangbyexample.com/tag/random/)*