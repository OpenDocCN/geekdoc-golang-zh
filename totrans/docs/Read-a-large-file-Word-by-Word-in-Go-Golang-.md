<!--yml
category: 未分类
date: 2024-10-13 06:01:42
-->

# Read a large file Word by Word in Go(Golang)

> 来源：[https://golangbyexample.com/read-large-file-word-by-word-go/](https://golangbyexample.com/read-large-file-word-by-word-go/)

When it comes to reading large files, obviously we don’t want to load the entire file in memory. bufio package in golang comes to the rescue when reading large files in Golang. Let’s say we have a sample.txt file with below contents

```
This is an example
to show how
to read file
word by word.
```

## **Word by Word scan**

```
package main
import (
    "bufio"
    "fmt"
    "log"
    "os"
)
func main() {
    WordbyWordScan()
}
func WordbyWordScan() {
    file, err := os.Open("./scanner/sample.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()
    scanner := bufio.NewScanner(file)
    scanner.Split(bufio.ScanWords)
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
This
is
an
example
to
show
how
to
read
file
line
by
line
and
word
by
word.
```

Please note that in the above program we set **scanner.Split(bufio.ScanWords)** which helps us to read the file word by word. Note however bufio.Scanner has max buffer size 64*1024 bytes which means in case you file has any line greater than the size of 64*1024, then it will give the error

```
bufio.Scanner: token too long
```