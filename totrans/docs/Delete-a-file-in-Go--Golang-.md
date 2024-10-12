<!--yml
category: 未分类
date: 2024-10-13 06:10:13
-->

# Delete a file in Go (Golang)

> 来源：[https://golangbyexample.com/delete-file-go/](https://golangbyexample.com/delete-file-go/)

**os.Remove()** function can be used to delete a file in Golang. Below is the signature of this function

```
func Remove(name string) error
```

**Code:**

```
package main

import (
    "log"
    "os"
)

func main() {
    err := os.Remove("sample.txt")
    if err != nil {
        log.Fatal(err)
    }
}
```

**Output:**

```
Deletes sample.txt file from the current working directory
```

*   [delete](https://golangbyexample.com/tag/delete/)*   [golang](https://golangbyexample.com/tag/golang/)*   [remove](https://golangbyexample.com/tag/remove/)