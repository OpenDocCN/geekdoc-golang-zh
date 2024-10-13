<!--yml

类别：未分类

日期：2024-10-13 06:38:13

-->

# Golang 正则表达式：在正则表达式中匹配数字或数值数字。

> 来源：[https://golangbyexample.com/golang-regex-match-number/](https://golangbyexample.com/golang-regex-match-number/)

目录

**   [概述](#Overview "Overview")

+   [匹配单个数字](#Match_a_single_digit "Match a single digit")

+   [匹配数字重复](#Match_Repetition_of_digit "Match Repetition of digit")*  *## **概述**

**\d** 可用于在 Golang 中匹配数字。实际上，**\d** 可用于匹配整个范围。

```
0-9
```

匹配任何数字的正则表达式将是。

```
\d
```

如果你只想匹配特定数字，比如说5，那么正则表达式就是那个数字。

```
5
```

如果你想匹配两个数字，那么下面是正则表达式。

```
\d\d
```

## **匹配单个数字**

让我们看一个示例。

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`\d`)

	fmt.Println("For regex \\d")
	match := sampleRegexp.MatchString("1")
	fmt.Printf("For 1: %t\n", match)

	match = sampleRegexp.MatchString("4")
	fmt.Printf("For 4: %t\n", match)

	match = sampleRegexp.MatchString("9")
	fmt.Printf("For 9: %t\n", match)

	match = sampleRegexp.MatchString("a")
	fmt.Printf("For a: %t\n", match)

	sampleRegexp = regexp.MustCompile(`5`)
	fmt.Println("\nFor regex 5")
	match = sampleRegexp.MatchString("5")
	fmt.Printf("For 5: %t\n", match)

	match = sampleRegexp.MatchString("6")
	fmt.Printf("For 6: %t\n", match)
}
```

**输出**

```
For regex \d
For 1: true
For 4: true
For 9: true
For a: false

For regex 5
For 5: true
For 6: false
```

在上述程序中，我们有两个正则表达式的示例。

+   **\d** – 匹配任何数字。

+   **5**  – 仅匹配数字五。

第一个匹配任何单个数字。这就是它的匹配原因。

```
1
4
9
```

它不会匹配。

```
a
```

从输出中也可以明显看出这一点。

第二个正则表达式只匹配 **“5”**，而不匹配 **“6”**，这从输出中可以明显看出。

```
For regex 5
For 5: true
For 6: false
```

## **匹配数字重复**

量词可用于匹配数字的重复。示例

+   **\d+** – 匹配一个或多个数字。

+   **\d*** – 匹配零个或多个数字。

+   **\d{N}** – 匹配 N 个数字。

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`\d+`)

	fmt.Println(`For regex \d+`)
	match := sampleRegexp.MatchString("12345")
	fmt.Printf("For 12345: %t\n", match)

	match = sampleRegexp.MatchString("")
	fmt.Printf("For empty string: %t\n", match)

	sampleRegexp = regexp.MustCompile(`\d*`)

	fmt.Println()
	fmt.Println(`For regex \d*`)
	match = sampleRegexp.MatchString("12345")
	fmt.Printf("For 12345: %t\n", match)

	match = sampleRegexp.MatchString("")
	fmt.Printf("For empty string: %t\n", match)

	sampleRegexp = regexp.MustCompile(`\d{2}`)

	fmt.Println()
	fmt.Println(`For regex \d{2}`)
	match = sampleRegexp.MatchString("12")
	fmt.Printf("For 12: %t\n", match)

	match = sampleRegexp.MatchString("1")
	fmt.Printf("For 1: %t\n", match)
}
```

**输出**

```
For regex \d+
For 12345: true
For empty string: false

For regex \d*
For 12345: true
For empty string: true

For regex \d{2}
For 12: true
For 1: false
```

在上述程序中，我们有三个正则表达式的示例。

+   **\d+**

+   **\d***

+   **\d{N}**

**\d+** 正则表达式会匹配 **“12345”**，但对空字符串失败。

**\d*** 会匹配 **“12345”** 以及一个空字符串。

**\d{2}** 匹配两个数字的序列。这就是为什么它能匹配 **“12”** 但对 **“1”** 失败。

此外，请查看我们的 Golang 高级教程系列 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)*
