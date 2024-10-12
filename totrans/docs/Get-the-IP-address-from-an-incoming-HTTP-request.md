<!--yml
category: 未分类
date: 2024-10-13 06:10:40
-->

# Get the IP address from an incoming HTTP request

> 来源：[https://golangbyexample.com/golang-ip-address-http-request/](https://golangbyexample.com/golang-ip-address-http-request/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

In this article, we will get the IP address of the client for an incoming HTTP request using

*   **X-REAL-IP** header

*   If **X-REAL-IP** is empty then we will fall back to **X-FORWARDED-FOR** header.

*   If **X-FORWARDED-FOR** is empty then we will fall back to **RemoteAddr** of http.Request struct

Please do note that

*   **X-REAL-IP** header contains only one IP address of the client machine. Some of the proxy servers such as Nginx populate this header if empty based upon the trust proxies that the request encountered before. Also please note that this header can easily be spoofed by the client.

*   **X-FORWARDED-FOR**  is a list of IP addresses – proxy chaining. Consider it as a request hop log. So if the request originates from the client having IP as **ip1**, and then hops through a proxy server with **ip2** and then goes through a load balancer having IP as **ip3**, then the value of **X-FORWARDED-FOR** will be **“ip1,ip2,ip3”** Therefore it is a good idea to split by **“,”**.  Also, note that it can also be easily spoofed by the client. You should only use this header if you control the proxy which is setting the header

*   **RemoteAddr** contains the real IP address of the client. It is the actual physical IP address that the web server received the connection from and that the response will be sent to. But in case the client is connected through a proxy it will give the IP address of the proxy. Also in case, you are using a load balancer or a reverse proxy server then it will give the address of them. **RemoteAddr** represents the IP port combination.

Please refer to this article for more information on X-REAL-IP and X-FORWARDED-FOR header

[https://distinctplace.com/2014/04/23/story-behind-x-forwarded-for-and-x-real-ip-headers/](https://distinctplace.com/2014/04/23/story-behind-x-forwarded-for-and-x-real-ip-headers/)

Out of the above 3 values, **RemoteAddr** is the most reliable but it will never give correct IP address if the client is behind the proxy or when using a load balancer or a reverse proxy server, therefore the order is first  **X-REAL-IP** then **X-FORWARDED-FOR**  and then **RemoteAddr.** Although please note that a malicious user can still create the fake **X-REAL-IP** and **X-FORWARDED-FOR** header

# **Code**

Let’s see a working code in GOLANG

```
package main

import (
    "fmt"
    "net"
    "net/http"
    "strings"
)

func main() {
    handlerFunc := http.HandlerFunc(handleRequest)
    http.Handle("/getIp", handlerFunc)
    http.ListenAndServe(":8080", nil)
}

func handleRequest(w http.ResponseWriter, r *http.Request) {
    ip, err := getIP(r)
    if err != nil {
        w.WriteHeader(400)
        w.Write([]byte("No valid ip"))
    }
    w.WriteHeader(200)
    w.Write([]byte(ip))
}

func getIP(r *http.Request) (string, error) {
    //Get IP from the X-REAL-IP header
    ip := r.Header.Get("X-REAL-IP")
    netIP := net.ParseIP(ip)
    if netIP != nil {
        return ip, nil
    }

    //Get IP from X-FORWARDED-FOR header
    ips := r.Header.Get("X-FORWARDED-FOR")
    splitIps := strings.Split(ips, ",")
    for _, ip := range splitIps {
        netIP := net.ParseIP(ip)
        if netIP != nil {
            return ip, nil
        }
    }

    //Get IP from RemoteAddr
    ip, _, err := net.SplitHostPort(r.RemoteAddr)
    if err != nil {
        return "", err
    }
    netIP = net.ParseIP(ip)
    if netIP != nil {
        return ip, nil
    }
    return "", fmt.Errorf("No valid ip found")
}
```

Make a curl call

```
curl -v -X  GET http://localhost:8080/getIp
```

**Output:**

```
On my machine it outputs ::1 which is actually loopback address for IPV6
```

On your machine, it will either output 127.0.0.1 for IPV4 or ::1 for IPV6

[https://en.wikipedia.org/wiki/Localhost](https://en.wikipedia.org/wiki/Localhost)

*   [golang](https://golangbyexample.com/tag/golang/)*