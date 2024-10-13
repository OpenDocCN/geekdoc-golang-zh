<!--yml

类别：未分类

日期：2024-10-13 06:52:56

-->

# Ruby 将字符串转换为布尔值

> 来源：[https://golangbyexample.com/string-bool-ruby/](https://golangbyexample.com/string-bool-ruby/)

目录

**[概述](#Overview "概述")**  # **概述**

在 Ruby 语言中，字符串“true”和“false”在 **if** 条件中被解释为 true。请参见下面的示例。

```go
if "false"
   puts "yes"
end
```

**输出**

```go
"yes"
```

“false” 计算为 true。

因此，正确处理字符串“false”或字符串“true”变得很重要。

我们可以创建一个自定义方法，根据字符串的内容返回布尔值 true 或 false

```go
def true?(str)
  str.to_s.downcase == "true"
end
```

我们可以尝试上述函数。

```go
true?("false")
 => false
```

```go
true?("true")
 => true
```

另外，请注意，对于除“false”以外的字符串，它将返回 true，但该函数可以轻松修改以处理这种情况。
