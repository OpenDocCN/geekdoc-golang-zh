<!--yml
category: 未分类
date: 2024-10-13 06:07:16
-->

# Iterate over all files and folders for a path in Go (Golang)

> 来源：[https://golangbyexample.com/iterate-over-all-files-and-folders-go/](https://golangbyexample.com/iterate-over-all-files-and-folders-go/)

**‘Walk’** function of ‘**filepath’** package can be used to recursively iterate over all files/folder in a directory tree.

```
https://golang.org/pkg/path/filepath/
```

**‘Walk’** function will walk the entire tree rooted at the root path include all subdirectories.  Below is the signature of the function

```
type WalkFunc func(path string, info os.FileInfo, err error) error
```

The WalkFunc will be called for with the **path** of the file/folder and **fileInfo** or the **error** if any error occurred while walking that file/folder.

Some things to note about Walk function

*   All errors are filtered. An error might arise while opening/visiting the file
*   The function does not follow symbolic links
*   The files are walked in lexical order

Let’s see an example:

```
package main

import (
    "fmt"
    "log"
    "os"
    "path/filepath"
)

func main() {
    currentDirectory, err := os.Getwd()
    if err != nil {
        log.Fatal(err)
    }
    iterate(currentDirectory)
}

func iterate(path string) {
    filepath.Walk(path, func(path string, info os.FileInfo, err error) error {
        if err != nil {
            log.Fatalf(err.Error())
        }
        fmt.Printf("File Name: %s\n", info.Name())
        return nil
    })
}
```