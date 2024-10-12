<!--yml
category: 未分类
date: 2024-10-13 06:17:42
-->

# Get ASCII code/value of any Alphabet or Number in Go (Golang)

> 来源：[https://golangbyexample.com/get-ascii-value-alphabet-go/](https://golangbyexample.com/get-ascii-value-alphabet-go/)

**range** over a string can give the **ASCII** of all the characters in the string. In below code we are printing the ASCII value of a lowercase alphabets, uppercase alphabets and numbers.

```
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

**Output:**

```
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

*   [alphabet](https://golangbyexample.com/tag/alphabet/)*   [ascii](https://golangbyexample.com/tag/ascii/)*   [code](https://golangbyexample.com/tag/code/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [lowercase](https://golangbyexample.com/tag/lowercase/)*   [number](https://golangbyexample.com/tag/number/)*   [uppercase](https://golangbyexample.com/tag/uppercase/)*   [value](https://golangbyexample.com/tag/value/)