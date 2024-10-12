<!--yml
category: 未分类
date: 2024-10-13 06:17:11
-->

# Rename file or folder in Go (Golang)

> 来源：[https://golangbyexample.com/rename-file-folder-golang/](https://golangbyexample.com/rename-file-folder-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")
    *   [Renaming a file](#Renaming_a_file "Renaming a file")
    *   [Renaming a folder](#Renaming_a_folder "Renaming a folder")*  *# **Overview**

os.Rename() function can be used to rename a file or folder. Below is the signature of the function.

```
func Rename(old, new string) error
```

**old** and **new** can be fully qualified as well. If the **old** and **new** path doesn’t lie in the same directory then **os.Rename()** function behaves the same as moving a file or folder.

# **Code**

## **Renaming a file**

Below is code for renaming a file

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

    err = os.Rename("temp.txt", "newTemp.txt")
    if err != nil {
        log.Fatal(err)
    }
}
```

**Output**

First, it will create a file named temp.txt in the current working directory. Then it will rename it to newTemp.txt

## **Renaming a folder**

Below is code for renaming a folder

```
package main

import (
    "log"
    "os"
)

func main() {
    //Create a directory
    err := os.Mkdir("temp", 0755)
    if err != nil {
        log.Fatal(err)
    }
    err = os.Rename("temp", "newTemp")
    if err != nil {
        log.Fatal(err)
    }
}
```

**Output:**

First it will create a folder named temp in current working directory. Then it will rename it to newTemp

*   [file](https://golangbyexample.com/tag/file/)*   [folder](https://golangbyexample.com/tag/folder/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [rename](https://golangbyexample.com/tag/rename/)*