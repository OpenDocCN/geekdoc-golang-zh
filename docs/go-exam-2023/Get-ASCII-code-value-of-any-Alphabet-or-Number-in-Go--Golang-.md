<!--yml

类别：未分类

日期：2024-10-13 06:17:42

-->

# 在Go (Golang)中获取任意字母或数字的ASCII代码/值

> 来源：[https://golangbyexample.com/get-ascii-value-alphabet-go/](https://golangbyexample.com/get-ascii-value-alphabet-go/)

**遍历**字符串可以获取字符串中所有字符的**ASCII**值。在下面的代码中，我们打印小写字母、大写字母和数字的ASCII值。

```go
package main

import "fmt"

func main() {
    lowercase := "abcdefghijklmnopqrstunwxyz"
    for _, c := range lowercase {
        fmt.Println(c)
    }

    uppercase := "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    for _, c := range uppercase {
        fmt.Println(c)
    }

    numbers := "0123456789"
    for _, n := range numbers {
        fmt.Println(n)
    }
}
```

**输出：**

```go
97
98
99
100
101
102
103
104
105
106
107
108
109
110
111
112
113
114
115
116
117
110
119
120
121
122
65
66
67
68
69
70
71
72
73
74
75
76
77
78
79
80
81
82
83
84
85
86
87
88
89
90
48
49
50
51
52
53
54
55
56
57
```

+   [字母](https://golangbyexample.com/tag/alphabet/)*   [ascii](https://golangbyexample.com/tag/ascii/)*   [代码](https://golangbyexample.com/tag/code/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [小写](https://golangbyexample.com/tag/lowercase/)*   [数字](https://golangbyexample.com/tag/number/)*   [大写](https://golangbyexample.com/tag/uppercase/)*   [值](https://golangbyexample.com/tag/value/)
