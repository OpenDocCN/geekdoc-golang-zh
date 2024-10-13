<!--yml

类别: 未分类

日期: 2024-10-13 06:00:06

-->

# Golang net/http 包 – 检测超时

> 来源：[`golangbyexample.com/net-http-timeout/`](https://golangbyexample.com/net-http-timeout/)

下面是一个示例代码，用于检测使用 net/http 包的上游调用是否存在超时。Http 客户端创建时设置为 1 纳秒超时，因此它在访问 google.com 时总是会超时。

```go
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

**输出:** true
