<!--yml

分类：未分类

日期：2024-10-13 06:42:44

-->

# 在Go语言中生成有效括号

> 来源：[https://golangbyexample.com/generate-valid-parentheses-golang/](https://golangbyexample.com/generate-valid-parentheses-golang/)

目录

**   [概述](#Overview "概述")*  *## **概述**

给定一个整数n，表示括号对的数量，生成所有有效的成对括号。

例如

```
Input:1
Output: [()]

Input:2
Output: [()(), (())]

Input:2
Output: [()()() ()(()) (())() (()()) ((()))]
```

**程序**的想法是使用两个整数

+   open – 它表示到目前为止使用的左括号的数量

+   close – 它表示到目前为止使用的右括号的数量

我们仅在

```
open < n //where n is the number of pairs of parentheses
```

我们仅在

```
close < open //Number of right parentheses is less than the number of left parentheses
```

以下是相应的程序

```
package main

import "fmt"

func main() {
	output := generateParenthesis(1)
	fmt.Println(output)

	output = generateParenthesis(2)
	fmt.Println(output)

	output = generateParenthesis(3)
	fmt.Println(output)
}

func generateParenthesis(n int) []string {
	input := ""
	for i := 0; i < n; i++ {
		input = input + "  "
	}
	output := generateParenthesisUtil(0, n, 0, 0, []rune(input))
	return output
}

func generateParenthesisUtil(pos, n, open, close int, input []rune) []string {

	var output []string
	if pos == n*2 {
		output = append(output, string(input))
		return output
	}

	if close < open {
		input[pos] = ')'
		result := generateParenthesisUtil(pos+1, n, open, close+1, input)
		output = append(output, result...)

	}

	if open < n {
		input[pos] = '('
		result := generateParenthesisUtil(pos+1, n, open+1, close, input)
		output = append(output, result...)
	}

	return output
}
```

**输出**

```
[()]
[()() (())]
[()()() ()(()) (())() (()()) ((()))]
```

**注意：** 查看我们的Golang高级教程。本系列教程内容详尽，我们尽力涵盖所有概念及示例。本教程适合那些希望掌握Golang并深入理解的读者 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对理解如何在Golang中实现所有设计模式感兴趣。如果是的话，这篇文章适合你 - [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [去](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
