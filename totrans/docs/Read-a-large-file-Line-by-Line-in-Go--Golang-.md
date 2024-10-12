<!--yml
category: 未分类
date: 2024-10-13 06:01:35
-->

# Read a large file Line by Line in Go (Golang)

> 来源：[https://golangbyexample.com/read-large-file-line-by-line-go/](https://golangbyexample.com/read-large-file-line-by-line-go/)

When it comes to reading large files, obviously we don’t want to load the entire file in memory. bufio package in golang comes to the rescue when reading large files. Let’s say we have a sample.txt file with below contents

```
This is an example
to show how
to read file 
line by line.
```

Here is the program:

```
package main
import (
    "bufio"
    "fmt"
    "log"
    "os"
)
func main(){
    LinebyLineScan()
}
func LinebyLineScan() {
    file, err := os.Open("./sample.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()
    scanner := bufio.NewScanner(file)
    for scanner.Scan() {
        fmt.Println(scanner.Text())
    }
    if err := scanner.Err(); err != nil {
        log.Fatal(err)
    }
}
```

**Output:**

```
This is an example
to show how 
to read file 
line by line.
```

Note however bufio.Scanner has max buffer size 64*1024 bytes which means in case you file has any line greater than the size of 64*1024, then it will give the error

```
bufio.Scanner: token too long
```

*   [file](https://golangbyexample.com/tag/file/)*   [go](https://golangbyexample.com/tag/go/)*   [largefile](https://golangbyexample.com/tag/largefile/)*   [line by line](https://golangbyexample.com/tag/line-by-line/)*   [linebyline](https://golangbyexample.com/tag/linebyline/)