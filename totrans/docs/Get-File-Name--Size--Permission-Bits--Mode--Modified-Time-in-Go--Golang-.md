<!--yml
category: 未分类
date: 2024-10-13 06:17:16
-->

# Get File Name, Size, Permission Bits, Mode, Modified Time in Go (Golang)

> 来源：[https://golangbyexample.com/file-info-golang/](https://golangbyexample.com/file-info-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

**os.Stat()** function can be used to the info of a file in go. This function returns stats which can be used to get

*   Name of the file

*   Size of the file in bytes

*   Modified Time of the file

*   Permission Bits or Mode of the file

Below is the signature of the function. It takes in the named file and return **FileInfo** struct which defines utility method to get above information

```
func Stat(name string) (FileInfo, error)
```

# **Code**

```
package main

import (
    "fmt"
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

    //Write something to the file
    file.WriteString("some sample text" + "\n")

    //Gets stats of the file
    stats, err := os.Stat("temp.txt")
    if err != nil {
        log.Fatal(err)
    }

    //Prints stats of the file
    fmt.Printf("Permission: %s\n", stats.Mode())
    fmt.Printf("Name: %s\n", stats.Name())
    fmt.Printf("Size: %d\n", stats.Size())
    fmt.Printf("Modification Time: %s\n", stats.ModTime())
}
```

**Output:**

```
Permission: -rwxrwxrwx
Name: temp.txt
Size: 17
Modification Time: 2020-04-16 22:26:47.080128602 +0530 IST
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [mode](https://golangbyexample.com/tag/mode/)*   [modified time](https://golangbyexample.com/tag/modified-time/)*   [permission bits](https://golangbyexample.com/tag/permission-bits/)*   [size](https://golangbyexample.com/tag/size/)*