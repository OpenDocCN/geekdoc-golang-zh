<!--yml
category: 未分类
date: 2024-10-13 06:16:56
-->

# Get Hostname in Go (Golang)

> 来源：[https://golangbyexample.com/get-hostname-golang/](https://golangbyexample.com/get-hostname-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

**‘os’** package of golang provides a **Hostname** function that can be used to get the hostname that is reported by the kernel
Below is the signature of this method. It returns an error if not able to successfully get the hostname

```
func Hostname() (name string, err error)
```

Let’s see a working code

# **Code**

```
package main

import (
	"fmt"
	"os"
)

func main() {
	hostname, err := os.Hostname()
	if err != nil {
		fmt.Println(err)
		os.Exit(1)
	}
	fmt.Printf("Hostname: %s", hostname)
}
```

**Output**:

```
Hostname: <hostname of="" your="" machine=""></hostname>
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [hostname](https://golangbyexample.com/tag/hostname/)*   [os](https://golangbyexample.com/tag/os/)*