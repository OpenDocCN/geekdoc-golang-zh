<!--yml
category: 未分类
date: 2024-10-13 06:09:38
-->

# Move file from one location to another in or command mv in Go (Golang)

> 来源：[https://golangbyexample.com/move-file-from-one-location-to-another-golang/](https://golangbyexample.com/move-file-from-one-location-to-another-golang/)

os.Rename() function can be used to mv or move a file from one location to another. It is equivalent to command **‘mv’** of Linux. Below is the signature of the function

```
func Rename(oldpath, newpath string) error
```

**Code:**

```
package main

import (
    "log"
    "os"
)

func main() {
    //Create a file
    file, err := os.Create("temp.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()

    //Change permission so that it can be moved
    err = os.Chmod("temp.txt", 0777)
    if err != nil {
        log.Println(err)
    }

    newLocation := "~/Desktop/temp.txt"
    err = os.Rename("temp.txt", newLocation)
    if err != nil {
        log.Fatal(err)
    }
}
```