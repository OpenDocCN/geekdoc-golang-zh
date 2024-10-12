<!--yml
category: 未分类
date: 2024-10-13 06:10:18
-->

# Delete/Remove a folder in Go (Golang)

> 来源：[https://golangbyexample.com/delete-folder-go/](https://golangbyexample.com/delete-folder-go/)

**os.Remove()** function can be used to delete a folder in Golang. Below is the signature of this function

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
    err := os.Remove("sample")
    if err != nil {
        log.Fatal(err)
    }
}
```

**Output:**

```
Deletes sample folder from the current working directory
```

*   [remove](https://golangbyexample.com/tag/remove/)