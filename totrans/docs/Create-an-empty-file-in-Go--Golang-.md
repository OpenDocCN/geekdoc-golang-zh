<!--yml
category: 未分类
date: 2024-10-13 06:09:20
-->

# Create an empty file in Go (Golang)

> 来源：[https://golangbyexample.com/create-empty-file-go/](https://golangbyexample.com/create-empty-file-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**os.Create()** can be used to create an empty file in go. The signature of the function is

```
func Create(name string) (*File, error) 
```

Basically this function

*   Create a named file with mode 0666
*   It truncates the file if it already exits
*   In case of path issue, it returns a Path error
*   It returns a file descriptor which can be used for both reading and write

# **Code:**

```
package main

import (
    "log"
    "os"
)

func main() {
    file, err := os.Create("emptyFile.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()
}
```

**Output**:

```
Check the contents of the file. It will be empty
```

*   [empty](https://golangbyexample.com/tag/empty/)*   [file](https://golangbyexample.com/tag/file/)*