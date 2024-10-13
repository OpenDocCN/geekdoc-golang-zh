<!--yml

类别：未分类

日期：2024-10-13 06:05:28

-->

# 如何在按下回车键之前暂停 Go 程序

> 来源：[https://golangbyexample.com/how-to-pause-a-go-program-until-enter-key-is-pressed/](https://golangbyexample.com/how-to-pause-a-go-program-until-enter-key-is-pressed/)

有时需要暂停 Go 程序，直到按下某个键。例如，只有在按下**回车**键时，才希望 Go 程序退出。

下面的程序会打印递增的数字，并会在你按下键时立即停止。试试看。

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    go counter()
    fmt.Println("Press the Enter Key to stop anytime")
    fmt.Scanln()
}

func counter() {
    i := 0
    for {
        fmt.Println(i)
        time.Sleep(time.Second * 1)
        i++
    }
}
```
