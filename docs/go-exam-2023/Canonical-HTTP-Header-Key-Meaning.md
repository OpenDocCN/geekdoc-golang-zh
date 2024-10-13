<!--yml

分类：未分类

日期：2024-10-13 06:31:38

-->

# 规范的 HTTP 头部键含义

> 来源：[https://golangbyexample.com/canonical-http-header-key/](https://golangbyexample.com/canonical-http-header-key/)

规范形式意味着第一个字符和其后任何跟随的连字符后的字符均为大写。其他所有字符均为小写。规范形式的示例有

```
Content-Type
Accept-Encoding
```

所有头部中的键都仅以规范形式表示。例如，如果在传入的 HTTP 请求中有以下头部

```
content-type: applcation/json
accept-encoding: gzip
```

然后在服务器端，不同语言的 HTTP 库大多会将其解释为规范形式。例如，当我们在服务器端打印头部时，上述头部将如下所示。

```
Content-Type: application/json
Accept-Encoding: gzip
```

注意

+   **content-type** 被转换为规范形式 **Content-Type**

+   **accept-encoding** 被转换为规范形式 **Accept-Encoding**
