<!--yml

类别：未分类

日期：2024-10-13 06:37:28

-->

# Golang正则表达式：替换所有匹配正则表达式的字符串

> 来源：[https://golangbyexample.com/regex-replace-string-golang/](https://golangbyexample.com/regex-replace-string-golang/)

目录

**   [概述](#Overview "概述")

+   [替换字符串作为字面字符串](#Replacement_string_as_a_literal_string "替换字符串作为字面字符串")

+   [一个包含未命名子匹配的捕获组的字符串](#A_string_having_capturing_group_unnamed_sub_matches "一个包含未命名子匹配的捕获组的字符串")

+   [一个包含命名子匹配的捕获组的字符串](#A_string_having_Capturing_groups_with_named_sub_matches "一个包含命名子匹配的捕获组的字符串")*  *## **概述**

Golang **regexp** 包提供了一种名为 **ReplaceAllString** 的方法，该方法可以用来替换与正则表达式匹配的字符串中的所有子字符串。

[https://golang.org/pkg/regexp/#Regexp.ReplaceAllString](https://golang.org/pkg/regexp/#Regexp.ReplaceAllString)

下面是该方法的签名

```go
func (re *Regexp) ReplaceAllString(src, repl string) string
```

它接受两个参数

+   第一个是输入字符串

+   第二个是替换字符串。

ReplaceAll返回**src**字符串的副本，将正则表达式的匹配项替换为替换字符串**repl**。

替换字符串可以是

+   一个字面字符串

+   一个包含未命名子匹配的捕获组的字符串。

+   一个包含命名子匹配的捕获组的字符串

听起来很混乱？当我们看到所有的示例时，一切都会变得清晰。让我们来看看它们

## **替换字符串作为字面字符串**

在这种情况下，替换字符串是一个字面字符串。下面是相同的示例。

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`\w+:[0-9]\d{1,3}`)

	input := "The persons are John:21, Simon:23, and Mike:19"

	result := sampleRegexp.ReplaceAllString(input, "redacted")
	fmt.Println(string(result))
}
```

**输出**

```go
The persons are redacted, redacted, and redacted
```

在上述示例中，我们有下面的正则表达式

```go
`\w+:[0-9]\d{1,3}`
```

它匹配形式的名称和年龄对

```go
name:age
```

然后我们有下面的输入字符串，它包含三个**name:age**对

```go
The persons are John:21, Simon:23, Mike:19
```

我们通过用屏蔽关键字替换它，删除了所有的**name:age**对。

```go
result := sampleRegexp.ReplaceAllString(input, "redacted")
```

请注意，替换字符串是一个字面字符串

```go
redacted
```

## **一个包含未命名子匹配的捕获组的字符串**

这是我们在正则表达式中有捕获组的情况。有关捕获组的详细信息，请参考这篇文章

在替换字符串中

+   **$1** 或 **${1}** 代表第一个子匹配

+   **$2** 或 **${2}** 代表第二个子匹配

+   …等等

让我们看下面的示例，这样会更清楚

```go
package main

import (
    "fmt"
    "regexp"
)

func main() {

    sampleRegexp := regexp.MustCompile(`(\w+):([0-9]\d{1,3})`)
    input := "The names are John:21, Simon:23, and Mike:19"

    result := sampleRegexp.ReplaceAllString(input, "$2:$1")
    fmt.Println(string(result))
}
```

**输出**

```go
The names are 21:John, 23:Simon, and 19:Mike
```

在上述示例中，我们有下面的正则表达式

```go
(\w+):([0-9]\d{1,3})
```

它与上面相同，匹配**name**和**age**对，但它有两个捕获组（用括号括起来），其中**$1**捕获名称，**$2**捕获年龄

+   **$0** 或 **${0}** – 完整匹配

```go
(\w+):([0-9]\d{1,3})
```

+   **$1** 或 **${1}** – 第一个子匹配

```go
(\w+)
```

+   **$2** -或 **${2}** – 第二个子匹配

```go
([0-9]\d{1,3})
```

然后我们有下面的输入字符串，它包含三个**name:age**对

```go
The names are John:21, Simon:23, and Mike:19
```

然后在替换字符串中，我们交换了顺序，使**age**先于**name**

```go
result := sampleRegexp.ReplaceAllString(input, "$2:$1")
```

这就是为什么在输出中我们先有**age**然后是**name**

```go
The names are 21:John, 23:Simon, and 19:Mike
```

你也可以只替换一个子匹配。例如，如果你只想遮蔽年龄，那也可以做到。见下面的程序。

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`(\w+):([0-9]\d{1,3})`)

	input := "The names are John:21, Simon:23, and Mike:19"

	result := sampleRegexp.ReplaceAllString(input, "$1")
	fmt.Println(string(result))
}
```

**输出**

```go
The names are John, Simon, and Mike
```

在上面的代码中，替换字符串是。

```go
$1
```

所以它用仅有的姓名替换整个**姓名:年龄**对。

你也可以使用**$0**。它代表整个匹配。

```go
package main

import (
    "fmt"
    "regexp"
)

func main() {
    sampleRegexp := regexp.MustCompile(`(\w+):([0-9]\d{1,3})`)
    input := "The names are John:21, Simon:23, and Mike:19"

    result := sampleRegexp.ReplaceAllString(input, "-$0-")
    fmt.Println(string(result))
}
```

**输出**

```go
The names are -John:21-, -Simon:23-, and -Mike:19-
```

在上面的示例中，我们在**$0**前后添加了**‘-‘**。

```go
-$0-
```

这就是为什么输出如上所示。

如果替换字符串包含字面美元符号，我们可以使用**ReplaceAllLiteralString**方法。

[https://golang.org/pkg/regexp/#Regexp.ReplaceAllLiteralString](https://golang.org/pkg/regexp/#Regexp.ReplaceAllLiteralString)

在这里，替换字符串直接被替代，没有任何分析，即被字面使用。见下面的示例。

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`(\w+):([0-9]\d{1,3})`)

	input := "The names are John:21, Simon:23, and Mike:19"

	result := sampleRegexp.ReplaceAllLiteralString(input, "$1")
	fmt.Println(string(result))
}
```

**输出**

```go
The names are $1, $1, and $1
```

如上所示，输出打印了字面上的美元符号。

## **一个包含命名子匹配的捕获组字符串**

这是当我们在正则表达式中有命名捕获组的情况。要了解捕获组的详细信息，请参阅这篇文章。

首先让我们看看一个程序，在这个程序中我们将看到命名捕获组。然后我们会详细分析这个程序以更清楚地理解。

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`(?P<name>\w+):(?P<age>[0-9]\d{1,3})`)

	input := "The names are John:21, Simon:23, Mike:19"

	result := sampleRegexp.ReplaceAllString(input, "$Age:$Name")
	fmt.Println(string(result))
}</age></name>
```

**输出**

```go
The names are 21:John, 23:Simon, 19:Mike
```

在上面的示例中，我们有以下正则表达式。

```go
`(?P<name>\w+):(?P<age>[0-9]\d{1,3})`</age></name>
```

它与上述的姓名和年龄配对相匹配，但有两个姓名捕获组。

+   第一个捕获组的名称是**“姓名”**。

```go
(?P<name>\w+)</name>
```

+   第二个捕获组的名称是**“年龄”**。

```go
(?P<age>[0-9]\d{1,3})</age>
```

然后我们有下面的输入字符串，它包含三个**姓名:年龄**对。

```go
The names are John:21, Simon:23 and Mike:19
```

然后在替换字符串中，我们交换了位置，使**年龄**在前，**姓名**在后。

```go
result := sampleRegexp.ReplaceAllString(input, "$Age:$Name")
```

这就是为什么在输出中我们先有**年龄**，然后是**姓名**。

```go
The names are 21:John, 23:Simon, 19:Mike
```

这就是关于在Golang中替换匹配正则表达式的字符串。希望你喜欢这篇文章。请在评论中分享反馈。

此外，查看我们的Golang进阶教程系列 – [Golang进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)。
