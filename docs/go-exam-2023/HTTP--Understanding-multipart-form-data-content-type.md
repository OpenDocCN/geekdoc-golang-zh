<!--yml

分类：未分类

日期：2024-10-13 06:33:41

-->

# HTTP - 理解multipart/form-data内容类型

> 来源：[https://golangbyexample.com/multipart-form-data-content-type-golang/](https://golangbyexample.com/multipart-form-data-content-type-golang/)

在HTTP上下文中，**multipart/form-data**内容类型用于提交HTML表单。在**multipart/form-data**的情况下，正如其名称所示，主体由不同部分组成，部分之间用分隔符或边界分隔，每个部分都有自己的头部描述。分隔符或边界也作为头部的一部分发送。

当你通过浏览器在HTTP调用中发送HTML表单时，数据内容可以以下两种格式发送为请求体。

+   application/x-www-form-urlencoded

+   multipart/form-data

在大多数情况下，可以使用**application/x-www-form-urlencoded**。**application/x-www-form-urlencoded**的效率不高

+   发送文件或图像

+   发送大量二进制数据或包含非ASCII字符的文本

例如，假设以下数据需要发送。

+   姓名

+   年龄

然后**application/x-www-form-urlencoded**可以用于发送上述数据。但假设你还需要在请求中发送用户的个人照片。那么数据现在如下

+   姓名

+   年龄

+   照片

在上述情况下，使用**application/x-www-form-urlencoded**内容类型效率不高。在这种情况下应使用**multipart/form-data**。因此，对于发送简单表单数据，请使用**application/x-www-form-urlencoded**，但如果表单数据还需要发送二进制数据，则请使用**multipart/form-data**

为什么会这样？在我们理解**application/x-www-form-urlencoded**和**multipart/form-data**的格式后，我们将回答这个问题。

**application/x-www-form-urlencoded**将每个非ASCII字节编码为3个字节。基本上，**application/x-www-form-urlencoded**内容类型请求体就像一个巨大的查询字符串。类似于URI中的查询字符串，它是一个具有以下格式的键值对

```go
key1=value1&key2=value21&key2=value22&key3=value3
```

发送**application/x-www-form-urlencoded**时，所有非字母数字字符都会被URL编码。所有非字母数字字符（仅限保留）将以以下格式进行URL编码

```go
%WW
```

其中**WW**是以十六进制格式表示的字母数字字符的ASCII码。由于二进制数据中的所有非字母数字字符都经过URL编码，1个字节转换为3个字节。因此，大小增加了三倍。因此，如果你发送的是一个文件或图像（这是一大堆二进制数据），那么你的负载将非常大，即几乎是实际负载的三倍。因此，发送大型二进制文件或大量非ASCII数据时效率不高。

要全面了解**application/x-www-form-urlencoded**的格式，请参考此链接

现在让我们理解**multipart/form-data**的格式。

正如我们之前提到的，**multipart/form-data** 有不同的部分由分隔符或边界分隔。每个部分都有自己的头部。multipart/form-data 的格式如下

```go
-- <delimiter_or_boundary>Content-Disposition: form-data; name="<key1>"
Content-Type: <content-type>

[DATA]
-- <delimiter_or_boundary>Content-Disposition: form-data; name="<key2>"; filename="<filename>"
Content-Type: <content-type>
[DATA]
--<delimiter_or_boundary>--</delimiter_or_boundary></content-type></filename></key2></delimiter_or_boundary></content-type></key1></delimiter_or_boundary>
```

如上所述，格式以分隔符或边界开始，并以分隔符或边界结束。上述格式分为两个部分。同时，

+   每个部分由分隔符或边界分隔。

+   每个部分包含自己的头部以描述数据的类型

+   每个部分的 Content-Disposition 头部将是 **form-data.** 包含名称字段。该字段包含键名。如果该部分是文件，还会有一个 **filename** 字段

+   每个部分也将包含自己的数据。

每个部分的 Content-Disposition 头部将是 **form-data.** 如果你发送的是二进制数据，那么

假设我们要将以下数据作为 **multipart/form-data** 请求的一部分发送

+   name = John

+   age = 23

+   photo = 一些二进制数据

假设分隔符或边界是

```go
xyz
```

那么格式将如下

```go
--xyz
Content-Disposition: form-data; name="name"
Content-Type: text/plain

John
--xyz
Content-Disposition: form-data; name="age"
Content-Type: text/plain

23
--xyz
Content-Disposition: form-data; name="photo"; filename="photo.jpeg"
Content-Type: image/jpeg

[JPEG DATA]
--xyz--
```

由于 **multipart/form-data** 将二进制数据按原样发送，这就是它用于发送文件和大二进制数据的原因。那么问题是，为什么不一直使用 form-data 呢？

原因是，对于小数据，额外的边界字符串和头部的要求会超过任何优化。例如，假设我们要发送以下数据

+   name=John

+   age=23

然后在使用 **application/x-www-form-urlencoded** 时，数据将被发送为

```go
name=John&age=23
```

但是在发送 **multipart/form-data** 时，数据将如下发送，几乎是以 **application/x-www-form-urlencoded** 发送的数据的十倍。

```go
--xyz
Content-Disposition: form-data; name="name"
Content-Type: text/plain

John
--xyz
Content-Disposition: form-data; name="age"
Content-Type: text/plain

23
--xyz--
```

**结论**

这就是关于 multipart/form-data 的所有内容。希望你喜欢这篇文章。请在评论中分享反馈
