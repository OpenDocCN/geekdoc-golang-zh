<!--yml

类别：未分类

日期：2024-10-13 06:32:33

-->

# HTTP 客户端或在 Go（Golang）中发送 x-www-form-urlencoded 请求主体

> 来源：[https://golangbyexample.com/http-client-urlencoded-body-go/](https://golangbyexample.com/http-client-urlencoded-body-go/)

目录

**   [概述](#Overview "Overview")

+   [示例](#Example "Example")*  *# **概述**

**application/x-www-form-urlencoded**内容类型的请求主体就像一个巨大的查询字符串。类似于 URI 中的查询字符串，它是一个键值对，具有以下格式。

```
key1=value1&key2=value21&key2=value22&key3=value3
```

下面是以下键值对

+   key1, value1

+   key2 有两个值，即 value21 和 value22。

+   key3, value3

每个键值对用 **&** 分隔，如果同一键有多个值，则会有两个条目。并且，每个键和值都经过 URL 编码，类似于查询字符串。

现在可能会有人问，如果**x-www-form-urlencoded**就像查询字符串，那它为什么存在。原因是查询字符串是 URI 的一部分，而 URI 的长度有限，因此你可以在查询字符串中发送有限数量的键值对。而**x-www-form-urlencoded**请求主体的长度没有限制。然而，它受到服务器允许的最大请求主体大小的限制，对于大多数服务器通常为10MB。现在让我们看看如何在 golang 中解析**x-www-form-urlencoded**

基本上，由于 x-www-form-urlencoded 主体与查询字符串类似，因此在 golang 中使用**url.Values**表示。[https://golang.org/pkg/net/url/#Values](https://golang.org/pkg/net/url/#Values)

**url.Values** 只不过是一个映射

+   key 是一个字符串

+   value 是一个字符串切片

它的表示如下

```
type Values map[string][]string
```

# **示例**

下面是相应的程序。这就是我们在程序中创建主体的方式

```
data := url.Values{}
data.Set("name", "John")
data.Set("age", "18")
data.Add("hobbies", "sports")
data.Add("hobbies", "music")
```

添加与设置的区别

+   **添加**将在**url.Values**映射中的键后追加一个额外值

+   **设置**将替换 **url.Values** 映射中现有的键

之后，我们需要对数据进行编码。然后编码后的数据传递给**http.NewRequest**函数。也不要忘记将内容类型头设置为**application/x-www-form-urlencoded**

```
encodedData := data.Encode()
http.NewRequest(method, urlPath, strings.NewReader(encodedData))
```

相应的完整程序。

```
package main

import (
	"fmt"
	"net/http"
	"net/url"
	"strconv"
	"strings"
	"time"
)

func main() {
	call("http://localhost:8080/employee", "POST")
}

func call(urlPath, method string) error {
	client := &http.Client{
		Timeout: time.Second * 10,
	}

	data := url.Values{}
	data.Set("name", "John")
	data.Set("age", "18")
	data.Add("hobbies", "sports")
	data.Add("hobbies", "music")
	encodedData := data.Encode()
	fmt.Println(encodedData)
	req, err := http.NewRequest(method, urlPath, strings.NewReader(encodedData))
	if err != nil {
		return fmt.Errorf("Got error %s", err.Error())
	}
	req.Header.Add("Content-Type", "application/x-www-form-urlencoded")
	req.Header.Add("Content-Length", strconv.Itoa(len(data.Encode())))
	response, err := client.Do(req)
	if err != nil {
		return fmt.Errorf("Got error %s", err.Error())
	}
	defer response.Body.Close()
	return nil
}
```

如果你运行一个监听在：8080 端口的服务器，类似于下面的链接，那么你会看到服务器能够正确打印传入请求的主体

[https://golangbyexample.com/url-encoded-body-golang](https://golangbyexample.com/url-encoded-body-golang)/

也是

```
fmt.Println(encodedData)
```

将会是

```
age=18&hobbies=sports&hobbies=music&name=John
```

它与我们上面讨论的格式相同。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
