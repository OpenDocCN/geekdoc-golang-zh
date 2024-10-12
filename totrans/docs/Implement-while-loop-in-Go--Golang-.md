<!--yml
category: 未分类
date: 2024-10-13 06:11:28
-->

# Implement while loop in Go (Golang)

> 来源：[https://golangbyexample.com/go-implement-while-loop/](https://golangbyexample.com/go-implement-while-loop/)

Go doesn’t have the **while** keyword. Instead, it has the **for** keyword only. However **for** keyword can be used to simulate the functionality the same as **while**.

**for** loop in GO basically has three parts:

```
for initialization_part; condition_part; increment_part {
   ...
}
```

**for** loop can be implemented to behave the same as **while** if **initialization_part** and **increment_part** can be skipped. Here is an example:

```
package main

import "fmt"

func main() {
    i := 1
    for i <= 5 {
        fmt.Println(i)
        i++
    }
}
```

**Output:**

```
1
2
3
4
5
```

*   [golang](https://golangbyexample.com/tag/golang/)