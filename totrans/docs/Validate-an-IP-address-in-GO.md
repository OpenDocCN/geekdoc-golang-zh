<!--yml
category: 未分类
date: 2024-10-13 06:08:58
-->

# Validate an IP address in GO

> 来源：[https://golangbyexample.com/validate-an-ip-address-in-go/](https://golangbyexample.com/validate-an-ip-address-in-go/)

ParseIP function of the net package can be used to validate an IP address. The same function can be used to validate both IPV4 and IPV6 addresses. Below is the signature of the function

```
func ParseIP(s string) IP
```

[https://golang.org/pkg/net/#ParseIP](https://golang.org/pkg/net/#ParseIP)

The ParseIP function

*   Returns nil if the given IP address is not valid
*   Else it creates an instance of IP struct and returns that.

Let’s see a working code:

```
package main

import (
    "fmt"
    "net"
)

func main() {
    validIPV4 := "10.40.210.253"

    checkIPAddress(validIPV4)

    invalidIPV4 := "1000.40.210.253"
    checkIPAddress(invalidIPV4)

    valiIPV6 := "2001:0db8:85a3:0000:0000:8a2e:0370:7334"
    checkIPAddress(valiIPV6)

    invalidIPV6 := "2001:0db8:85a3:0000:0000:8a2e:0370:7334:3445"
    checkIPAddress(invalidIPV6)
}

func checkIPAddress(ip string) {
    if net.ParseIP(ip) == nil {
        fmt.Printf("IP Address: %s - Invalid\n", ip)
    } else {
        fmt.Printf("IP Address: %s - Valid\n", ip)
    }
} 
```

**Output:**

```
IP Address: 10.40.210.253 - Valid
IP Address: 1000.40.210.253 - Invalid
IP Address: 2001:0db8:85a3:0000:0000:8a2e:0370:7334 - Valid
IP Address: 2001:0db8:85a3:0000:0000:8a2e:0370:7334:3445 - Invalid
```

*   [ipv6](https://golangbyexample.com/tag/ipv6/)