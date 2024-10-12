<!--yml
category: 未分类
date: 2024-10-13 06:09:02
-->

# Check if an IP address is IPV4 or IPV6 in Go (Golang)

> 来源：[https://golangbyexample.com/check-ip-address-is-ipv4-or-ipv6-go/](https://golangbyexample.com/check-ip-address-is-ipv4-or-ipv6-go/)

*   IPV4 address is of 4 bytes string with each byte separated by dot (‘.’)

*   IPV6 address is 8 groups of 4 hexagonal digits separated by colon (‘:’)

The below code

*   Prints invalid if the given IP address is invalid
*   Else it prints whether the IP address is ipV4 or ipV6

**Code:**

```
package main

import (
    "fmt"
    "net"
)

func main() {
    validIPV4 := "10.40.210.253"
    checkIPAddressType(validIPV4)

    invalidIPV4 := "1000.40.210.253"
    checkIPAddressType(invalidIPV4)

    valiIPV6 := "2001:0db8:85a3:0000:0000:8a2e:0370:7334"
    checkIPAddressType(valiIPV6)

    invalidIPV6 := "2001:0db8:85a3:0000:0000:8a2e:0370:7334:3445"
    checkIPAddressType(invalidIPV6)
}

func checkIPAddressType(ip string) {
    if net.ParseIP(ip) == nil {
        fmt.Printf("Invalid IP Address: %s\n", ip)
        return
    }
    for i := 0; i < len(ip); i++ {
        switch ip[i] {
        case '.':
            fmt.Printf("Given IP Address %s is IPV4 type\n", ip)
            return
        case ':':
            fmt.Printf("Given IP Address %s is IPV6 type\n", ip)
            return
        }
    }
}
```

**Output:**

```
Given IP Address 10.40.210.253 is IPV4 type
Invalid IP Address: 1000.40.210.253
Given IP Address 2001:0db8:85a3:0000:0000:8a2e:0370:7334 is IPV6 type
Invalid IP Address: 2001:0db8:85a3:0000:0000:8a2e:0370:7334:3445
```