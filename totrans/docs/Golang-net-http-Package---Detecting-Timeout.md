<!--yml
category: 未分类
date: 2024-10-13 06:00:06
-->

# Golang net/http Package – Detecting Timeout

> 来源：[https://golangbyexample.com/net-http-timeout/](https://golangbyexample.com/net-http-timeout/)

Below is a sample code piece to know if there is a timeout for an upstream call that is using net/http package. Http Client is created with 1 nanosecond timeout so that it always times out on google.com

```
package main

import (
	"fmt"
	"net/http"
	"os"
	"time"
)

func main() {

	client := &http.Client{
		Timeout: time.Nanosecond * 1,
	}

	_, err := client.Get("https://google.com")

	isTimeout := false
	if os.IsTimeout(err) {
		isTimeout = true
	}

	fmt.Println("isTimeout)
}
```

**Output:** true