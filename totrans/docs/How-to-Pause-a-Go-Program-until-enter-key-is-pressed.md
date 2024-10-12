<!--yml
category: 未分类
date: 2024-10-13 06:05:28
-->

# How to Pause a Go Program until enter key is pressed

> 来源：[https://golangbyexample.com/how-to-pause-a-go-program-until-enter-key-is-pressed/](https://golangbyexample.com/how-to-pause-a-go-program-until-enter-key-is-pressed/)

Sometimes there is a need to pause a go program until a key is pressed. Eg. One would only want a go program to exit at the end only on the press of **Enter** key.

Below program will print the increasing and will stop as soon as you press the key is pressed. Try it out.

```
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