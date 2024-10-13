<!--yml

类别：未分类

日期：2024-10-13 06:32:08

-->

# 获取Go（Golang）中发出的HTTP请求的响应头。

> 来源：[https://golangbyexample.com/get-response-headers-making-go/](https://golangbyexample.com/get-response-headers-making-go/)

注意：相关帖子

+   从传入的HTTP请求中获取请求头，在Golang中 – [https://golangbyexample.com/headers-http-request-golang/](https://golangbyexample.com/headers-http-request-golang/)

+   为传入的HTTP请求设置响应头，在Golang中 – [https://golangbyexample.com/set-resposne-headers-http-go/](https://golangbyexample.com/set-resposne-headers-http-go/)

+   为发出的HTTP请求设置请求头 – [https://golangbyexample.com/set-headers-http-request/](https://golangbyexample.com/set-headers-http-request/)

现在让我们看看在进行HTTP请求时如何获取头部信息。

目录

**   [概述](#Overview "Overview")

+   [使用 Header.Values 方法](#Using_HeaderValues_method "Using Header.Values method")

    +   [使用 Header.Get 方法](#Using_HeaderGet_method "Using Header.Get method")

+   [直接访问 Header 结构](#Directly_Accessing_the_Header_Struct "直接访问 Header 结构")

+   [示例](#Example "Example")*  *# **概述**

以下是Go中表示头部的格式。

```
type Header map[string][]string
```

所以头部是一个键值对，

+   键以规范形式表示。规范形式意味着第一个字符和任何在连字符后面的字符都是大写。其他所有字符都是小写。规范形式的示例包括

```
Content-Type
Accept-Encoding
```

值表示为字符串切片。为什么是字符串数组？因为在请求中允许有两个相同键但不同值的头部。这两个值会被收集到切片中。

HTTP请求中的响应由**http.Response**结构表示。

[https://golang.org/src/net/http/response.go](https://golang.org/src/net/http/response.go)

在**http.Response**结构中定义了一个**Header**字段，如下所示。它包含与响应相关的响应头。

```
type Response interface {
    Header() Header
    ...
}
```

在下面的示例中，假设变量**w**的类型是**http.Response**。现在让我们看看获取响应头的不同方法。此外，假设我们在发出的请求中目前有以下响应头。

```
User-Agent: application/json
Foo: bar1
Foo: bar2
```

# **使用 Header.Values 方法**

用于访问给定键的头部的所有值，使用Values方法。下面是该方法的格式。

```
func (h Header) Values(key string) []string
```

请注意，返回类型是字符串切片。它将返回与给定头部键相关的所有值。此外，在获取值之前，给定键会先转换为规范形式。可以如下调用

```
w.Header.Values(key)
```

例如

+   **w.Header.Values(“content-type”)** 将输出 **[“application/json” ]**

+   **w.Header.Values(“foo”)** 将输出 **[“bar1”, “bar2”]**

## **使用 Header.Get 方法**

用于根据给定键访问头部的第一个值。这是最常用的方法，因为通常与头部关联的值只有一个。以下是该方法的格式。

```
func (h Header) Get(key string) string
```

注意返回类型只是一个字符串。它将返回与给定键关联的第一个值。此外，在获取值之前，给定的键首先会转换为规范形式。可以如下调用：

```
w.Header.Get(key)
```

例如对于

+   **w.Header.Get(“content-type”)** 将输出 **“application/json”**

+   **w.Header.Get(“foo”)** 将输出 **“bar1”**。只会返回第一个值。

# **直接访问 Header 结构**

Header 映射也可以直接访问，它存在于 *http.Response* 中。

例如

+   **w.Header** 将输出一个映射

```
map[Accept:[*/*] Content-Type:[application/json] Foo:[bar1 bar2]
```

+   **w.Header[“Content-Type”]** 将输出

```
[application/json]
```

+   **w.Header[“Foo”]** 将输出

```
[bar1 bar2]
```

请注意，直接访问 Header 映射时，我们需要仅以规范形式提供键。

# **示例**

让我们看一个展示以上所有要点的程序

```
package main

import (
	"fmt"
	"net/http"
	"time"
)

func main() {
	call("https://google.com", "GET")
}

func call(url, method string) error {
	client := &http.Client{
		Timeout: time.Second * 10,
	}
	req, err := http.NewRequest(method, url, nil)
	if err != nil {
		return fmt.Errorf("Got error %s", err.Error())
	}
	res, err := client.Do(req)
	if err != nil {
		return fmt.Errorf("Got error %s", err.Error())
	}
	defer res.Body.Close()

	contentTypeValues := res.Header.Values("content-type")
	fmt.Printf("res.Header.Values(\"content-type\"):: %s\n\n", contentTypeValues)

	contentType := res.Header.Get("content-type")
	fmt.Printf("res.Header.Get(\"content-type\"):: %s\n\n", contentType)

	headers := res.Header
	fmt.Printf("res.Headers:: %s\n\n", headers)
	fmt.Printf("res.Headers[\"Content-Type\"]:: %s\n\n", headers["Content-Type"])
	return nil

}
```

运行上述程序后，检查终端中的输出。结果与我们讨论的完全一致。以下是输出：

```
res.Header.Values("content-type"):: 

res.Header.Get("content-type"):: text/html; charset=ISO-8859-1

res.Headers:: map[Alt-Svc:[h3-29=":443"; ma=2592000,h3-T051=":443"; ma=2592000,h3-Q050=":443"; ma=2592000,h3-Q046=":443"; ma=2592000,h3-Q043=":443"; ma=2592000,quic=":443"; ma=2592000; v="46,43"] Cache-Control:[private, max-age=0] Content-Type: Date:[Thu, 10 Dec 2020 16:38:03 GMT] Expires:[-1] P3p:[CP="This is not a P3P policy! See g.co/p3phelp for more info."] Server:[gws] Set-Cookie:[1P_JAR=2020-12-10-16; expires=Sat, 09-Jan-2021 16:38:03 GMT; path=/; domain=.google.com; Secure NID=204=w6zf-xFyVywx7QaClDZuQ5N-Yc-4HqKWBS-JXWp2Tat9kmq0BRsanM35PJHiM2iEn4TbP2HcTUd0KkIuMuIW7xFewD5un2_mc0O4fm2IXzrQyRmPWHJSeQJUUVb0-_lIfJgSnGmJm2MptRsd2egrPsbZJQBZWd97o7KlFBI3CIE; expires=Fri, 11-Jun-2021 16:38:03 GMT; path=/; domain=.google.com; HttpOnly] X-Frame-Options:[SAMEORIGIN] X-Xss-Protection:[0]]

res.Headers["Content-Type"]:: 
```

注意

```
res.Header.Values("content-type"):
```

输出一个数组

````while    ``` res.Header.Get("content-type") ```    outputs    ``` text/html; charset=ISO-8859-1 ```    Also    ``` res.Headers ```    outputs the entire header map. Notice that each key is in the canonical format    ``` map[Alt-Svc:[h3-29=":443"; ma=2592000,h3-T051=":443"; ma=2592000,h3-Q050=":443"; ma=2592000,h3-Q046=":443"; ma=2592000,h3-Q043=":443"; ma=2592000,quic=":443"; ma=2592000; v="46,43"] Cache-Control:[private, max-age=0] Content-Type: Date:[Thu, 10 Dec 2020 16:38:03 GMT] Expires:[-1] P3p:[CP="This is not a P3P policy! See g.co/p3phelp for more info."] Server:[gws] Set-Cookie:[1P_JAR=2020-12-10-16; expires=Sat, 09-Jan-2021 16:38:03 GMT; path=/; domain=.google.com; Secure NID=204=w6zf-xFyVywx7QaClDZuQ5N-Yc-4HqKWBS-JXWp2Tat9kmq0BRsanM35PJHiM2iEn4TbP2HcTUd0KkIuMuIW7xFewD5un2_mc0O4fm2IXzrQyRmPWHJSeQJUUVb0-_lIfJgSnGmJm2MptRsd2egrPsbZJQBZWd97o7KlFBI3CIE; expires=Fri, 11-Jun-2021 16:38:03 GMT; path=/; domain=.google.com; HttpOnly] X-Frame-Options:[SAMEORIGIN] X-Xss-Protection:[0]] ```  *   [golang](https://golangbyexample.com/tag/golang/)````*
