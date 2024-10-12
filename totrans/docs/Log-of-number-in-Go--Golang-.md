<!--yml
category: 未分类
date: 2024-10-13 06:15:17
-->

# Log of number in Go (Golang)

> 来源：[https://golangbyexample.com/log-of-number-go-golang/](https://golangbyexample.com/log-of-number-go-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Natural logarithm](#Natural_logarithm "Natural logarithm")
*   [Code](#Code "Code")
*   [Binary Exponent Log (log e)](#Binary_Exponent_Log_log_e "Binary Exponent Log (log e)")
    *   [Code](#Code-2 "Code")
*   [Binary Log (log 2)](#Binary_Log_log_2 "Binary Log (log 2)")
    *   [Code](#Code-3 "Code")
*   [Decimal Log (log 10)](#Decimal_Log_log_10 "Decimal Log (log 10)")
    *   [Code](#Code-4 "Code")*  *# **Overview**

In this tutorial, we will see three types of logarithm possible

*   Natural logarithm

*   Binary Exponent Log (log e)

*   Binary Log (log 2)

*   Decimal Log (log 10)

# Natural logarithm

**math** package of GO provides a **Log** method that can be used to get the natural logarithm of a number

Below is the signature of the function. It takes input as a **float64** number and returns a **float64**.

```
func Log(x float64) float64
```

Also some special cases of **Logb** function are

*   Log(±Inf) = +Inf
*   Log(0) = -Inf
*   Log(NaN) = NaN
*   Log(x < 0) = NaN

# **Code**

```
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Log(4)
    fmt.Println(res)

    res = math.Log(10.2)
    fmt.Println(res)

    res = math.Log(-10)
    fmt.Println(res)
}
```

**Output:**

```
1.3862943611198906
2.322387720290225
NaN
```

# **Binary Exponent Log (log e)**

**math** package of golang provides a **Logb** method that can be used to get the binary exponent of a number

Below is the signature of the function. It takes input as a **float64** number and returns a **float64**.

```
func Logb(x float64) float64
```

Also some special cases of **Logb** function are

*   Logb(±Inf) = +Inf
*   Logb(0) = -Inf
*   Logb(NaN) = NaN

## **Code**

```
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Logb(4)
    fmt.Println(res)

    res = math.Logb(10.2)
    fmt.Println(res)

    res = math.Logb(-10)
    fmt.Println(res)
}
```

**Output:**

```
2
3
3
```

# **Binary Log (log 2)**

**math** package of golang provides a **Log2** method that can be used to get the binary logarithm or log to base 2 of a number

Below is the signature of the function. It takes input as a float64 number and returns a float64\.

Also some special cases of **Log2** function are

*   **Log2**(±Inf) = +Inf
*   **Log2**(0) = -Inf
*   **Log2**(NaN) = NaN
*   **Log2**(x < 0) = NaN

## **Code**

```
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Log2(4)
    fmt.Println(res)

    res = math.Log2(10.2)
    fmt.Println(res)

    res = math.Log2(-10)
    fmt.Println(res)
}
```

**Output:**

```
2
3.321928094887362
NaN
```

# **Decimal Log (log 10)**

**math** package of go provides a **Log10** method that can be used to get the decimal logarithm or log to base 10 of a number

Below is the signature of the function. It takes input as a float64 number and returns a float64\.

```
func Log10(x float64) float64
```

Also some special cases of **Log10** function are

*   Log10(±Inf) = +Inf
*   Log10(0) = -Inf
*   Log10(NaN) = NaN
*   Log10(x < 0) = NaN

## **Code**

```
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Log10(100)
    fmt.Println(res)

    res = math.Log10(10)
    fmt.Println(res)

    res = math.Log10(-10)
    fmt.Println(res)
}
```

**Output:**

```
2
1
NaN
```

*   [go](https://golangbyexample.com/tag/go/)*   [log](https://golangbyexample.com/tag/log/)*   [log 10](https://golangbyexample.com/tag/log-10/)*   [log 2](https://golangbyexample.com/tag/log-2/)*   [log e](https://golangbyexample.com/tag/log-e/)*