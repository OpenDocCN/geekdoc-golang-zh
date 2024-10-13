<!--yml

category: 未分类

date: 2024-10-13 06:08:58

-->

# 在 GO 中验证 IP 地址

> 来源：[https://golangbyexample.com/validate-an-ip-address-in-go/](https://golangbyexample.com/validate-an-ip-address-in-go/)

net 包的 ParseIP 函数可用于验证 IP 地址。该函数可以同时验证 IPV4 和 IPV6 地址。以下是该函数的签名。

```go
func ParseIP(s string) IP
```

[https://golang.org/pkg/net/#ParseIP](https://golang.org/pkg/net/#ParseIP)

ParseIP 函数

+   如果给定的 IP 地址无效，则返回 nil。

+   否则它会创建一个 IP 结构的实例并返回该实例。

让我们看看一个可运行的代码：

```go
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

**输出：**

```go
IP Address: 10.40.210.253 - Valid
IP Address: 1000.40.210.253 - Invalid
IP Address: 2001:0db8:85a3:0000:0000:8a2e:0370:7334 - Valid
IP Address: 2001:0db8:85a3:0000:0000:8a2e:0370:7334:3445 - Invalid
```

+   [ipv6](https://golangbyexample.com/tag/ipv6/)
