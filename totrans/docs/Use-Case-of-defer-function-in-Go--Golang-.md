<!--yml
category: 未分类
date: 2024-10-13 06:26:42
-->

# Use Case of defer function in Go (Golang)

> 来源：[https://golangbyexample.com/defer-use-case-go/](https://golangbyexample.com/defer-use-case-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

Defer as the name suggests is used to defer the cleanup activities in a function. These cleanup activities will be performed at the end of the function. This cleanup activities will be done in a different function called by defer.  This different function is called at the end of the surrounding function before it returns. Below is the syntax of defer function.

```
defer {function_or_method_call}
```

One good example of understanding the use case defer function is to look at the use case of writing to a file. A file that is opened for writing also must to be closed.   

# **Example**

```
package main

import (
    "fmt"
    "log"
    "os"
)

func main() {
    err := writeToTempFile("Some text")
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Printf("Write to file succesful")
}

func writeToTempFile(text string) error {
    file, err := os.Open("temp.txt")
    if err != nil {
        return err
    }
    n, err := file.WriteString("Some text")
    if err != nil {
        return err
    }
    fmt.Printf("Number of bytes written: %d", n)
    file.Close()
    return nil
}
```

In the above program, in the **writeToTempFile** function, we are opening a file and then trying to write some content to the file. After we have written the contents of the file we close the file. It is possible that during the write operation it might result into an error and function will return without closing the file. **Defer** function helps to avoid these kinds of problems. **Defer** function is always executed before the surrounding function returns. Let’s rewrite the above program with a **defer** function here.

```
package main

import (
    "fmt"
    "log"
    "os"
)

func main() {
    err := writeToTempFile("Some text")
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Printf("Write to file succesful")
}

func writeToTempFile(text string) error {
    file, err := os.Open("temp.txt")
    if err != nil {
        return err
    }
    defer file.Close()

    n, err := file.WriteString("Some text")
    if err != nil {
        return err
    }
    fmt.Printf("Number of bytes written: %d", n)
    return nil
}
```

In the above program, we do **defer file.Close()** after opening the file. This will make sure that closing of the file is executed even if the write to the file results into an error. Defer function makes sure that the file will be closed regardless of number of return statements in the function

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*