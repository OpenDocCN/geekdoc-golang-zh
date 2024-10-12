<!--yml
category: 未分类
date: 2024-10-13 06:07:29
-->

# Append to an existing file in Go (Golang)

> 来源：[https://golangbyexample.com/append-file-golang/](https://golangbyexample.com/append-file-golang/)

**os.OpenFile()** function of the os package can be used to open to a file in an append mode and then write to it

Let’s see an example. In the below program:

*   First, write to a file using **ioutil** package
*   Open the file again in Append mode and write the second line to it
*   Read the file to verify the content.

```
package main

import (
    "fmt"
    "io/ioutil"
    "log"
    "os"
)

func main() {
    //Write first line
    err := ioutil.WriteFile("temp.txt", []byte("first line\n"), 0644)
    if err != nil {
        log.Fatal(err)
    }

    //Append second line
    file, err := os.OpenFile("temp.txt", os.O_APPEND|os.O_WRONLY, 0644)
    if err != nil {
        log.Println(err)
    }
    defer file.Close()
    if _, err := file.WriteString("second line"); err != nil {
        log.Fatal(err)
    }

    //Print the contents of the file
    data, err := ioutil.ReadFile("temp.txt")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(string(data))
}
```

**Output:**

```
first line
second line
```

*   [file](https://golangbyexample.com/tag/file/)*   [go](https://golangbyexample.com/tag/go/)