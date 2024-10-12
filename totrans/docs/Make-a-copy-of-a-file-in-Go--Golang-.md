<!--yml
category: 未分类
date: 2024-10-13 06:17:27
-->

# Make a copy of a file in Go (Golang)

> 来源：[https://golangbyexample.com/copy-file-go/](https://golangbyexample.com/copy-file-go/)

In this article we will see two methods of copying a file.

Table of Contents

 **   [First Method](#First_Method "First Method")
*   [Second Method](#Second_Method "Second Method")*  *## **First Method**

**io.Copy()** can be used to create a copy of the file from src to dest. A successful copy will return err != nil instead of err == EOF

Below is the signature of the function. It returns the number of bytes written to the dest

```
func Copy(dst Writer, src Reader) (written int64, err error)
```

**Code**

First create a file named **“original.txt”** and write some content in it.

```
package main

import (
    "fmt"
    "io"
    "log"
    "os"
)

// Copy a file
func main() {
    // Open original file
    original, err := os.Open("original.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer original.Close()

    // Create new file
    new, err := os.Create("new.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer new.Close()

    //This will copy
    bytesWritten, err := io.Copy(new, original)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("Bytes Written: %d\n", bytesWritten)
}
```

**Output:**

```
Bytes Written: 
```

## **Second Method**

If the contents of the file is less then we can also read the contents of the file first and then write all the contents to the new file. See below code.

Here also first create a file named **“original.txt**” and write some content in it

```
package main
import (
    "io/ioutil"
    "log"
)

// Copy a file
func main() {
    //Read all the contents of the  original file
    bytesRead, err := ioutil.ReadFile("original.txt")
    if err != nil {
        log.Fatal(err)
    }

    //Copy all the contents to the desitination file
    err = ioutil.WriteFile("new.txt", bytesRead, 0755)
    if err != nil {
        log.Fatal(err)
    }
}
```

**Output**

```
new.txt file will have same content as original.txt
```

*   [copy](https://golangbyexample.com/tag/copy/)*   [file](https://golangbyexample.com/tag/file/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*