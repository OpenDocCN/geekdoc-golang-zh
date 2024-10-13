<!--yml

分类：未分类

日期：2024-10-13 06:13:01

-->

# 在Go (Golang)中用另一个子字符串替换一些子字符串的实例

> 来源：[https://golangbyexample.com/replace-some-instances-substring-go/](https://golangbyexample.com/replace-some-instances-substring-go/)

目录

**   [概述](#Overview "概述")

+   [代码：](#Code "代码：")*  *# **概述**

在GO中，字符串是UTF-8编码的。GO的**strings**包提供了一个**Replace**方法，可用于将给定子字符串的一些不重叠实例替换为新子字符串。它返回字符串的副本。

下面是函数的签名。

```
func Replace(s, old, new string, n int)
```

+   该函数将在字符串**s**中将所有不重叠的**old**实例替换为**new**。

+   如果**old**为空，则在字符串**s**中的每个有效UTF-8字节序列之间插入**new**。

+   如果**n**为负，则所有**old**的实例将被替换为**new**。

让我们看看工作代码。

# **代码：**

```
package main

import (
    "fmt"
    "strings"
)

func main() {
    res := strings.Replace("abcdabxyabr", "ab", "12", 1)
    fmt.Println(res)

    res = strings.Replace("abcdabxyabr", "ab", "12", -1)
    fmt.Println(res)
}
```

**输出：**

```
12cdabxyabr
12cd12xy12r
```*
