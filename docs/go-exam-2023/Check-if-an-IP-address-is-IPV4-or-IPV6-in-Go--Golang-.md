<!--yml

分类：未分类

日期：2024-10-13 06:09:02

-->

# 在Go（Golang）中检查IP地址是IPV4还是IPV6

> 来源：[https://golangbyexample.com/check-ip-address-is-ipv4-or-ipv6-go/](https://golangbyexample.com/check-ip-address-is-ipv4-or-ipv6-go/)

+   IPV4地址是4字节字符串，每个字节用点（‘.’）分隔

+   IPV6地址由8组4个十六进制数字组成，每组用冒号（‘：’）分隔

以下代码

+   如果给定的IP地址无效，则打印无效

+   否则打印该IP地址是ipV4还是ipV6

**代码：**

```go
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

**输出：**

```go
Given IP Address 10.40.210.253 is IPV4 type
Invalid IP Address: 1000.40.210.253
Given IP Address 2001:0db8:85a3:0000:0000:8a2e:0370:7334 is IPV6 type
Invalid IP Address: 2001:0db8:85a3:0000:0000:8a2e:0370:7334:3445
```
