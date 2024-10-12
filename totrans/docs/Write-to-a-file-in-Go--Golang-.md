<!--yml
category: 未分类
date: 2024-10-13 06:06:54
-->

# Write to a file in Go (Golang)

> 来源：[https://golangbyexample.com/write-to-a-file-go/](https://golangbyexample.com/write-to-a-file-go/)

There are different ways of writing a file in GO. Let’s examine this in detail through different examples which illustrates how to write line by line to a file.

Table of Contents

 **   [Buffered write using bufio package](#Buffered_write_using_bufio_package "Buffered write using bufio package")
    *   [Example: With Default Buffer Size of 4096 bytes](#Example_With_Default_Buffer_Size_of_4096_bytes "Example: With Default Buffer Size of 4096 bytes")
    *   [Example: With Custom Buffer Size of 10 bytes](#Example_With_Custom_Buffer_Size_of_10_bytes "Example: With Custom Buffer Size of 10 bytes")
*   [Using file.Write()](#Using_fileWrite "Using file.Write()")
*   [Using ioutil.WriteFile()](#Using_ioutilWriteFile "Using ioutil.WriteFile()")*  *# **Buffered write using bufio package**

**bufio** package can be used to do a buffered write in Go. It has a default buffer size of 4096 bytes however a custom buffer size can also be specified. The main advantage of buffered write is that it keeps the data to be written in the buffer and thus minimizes the number of times we have to do the IO operation. It does a write when

*   **writer.flush()** is called
*   Buffer is full on next write. For example, let’s say it has 3500 bytes in full in buffer and buffer has a size of 4096\. The next line to be written is of 1000 bytes. Since it 3500 + 1000 > 4096, it will write some of the bytes to file so that some buffer is available for next write.

If there is no buffer then there will be IO write on every line. Let’s see an example of a write to a file. We are using **writer.Available()** and **writer.Buffered()** to print the **available** and **used** buffer size respectively.

## **Example: With Default Buffer Size of 4096 bytes**

It will write to **temp.txt** in the current directory. Write happens at the end when **writer.Flush()** is called as buffer never really gets full in between.

```
package main
import (
    "bufio"
    "fmt"
    "log"
    "os"
)

func main() {
    file, err := os.Create("./temp.txt")
    if err != nil {
        log.Fatal(err)
    }
    writer := bufio.NewWriter(file)
    linesToWrite := []string{"This is an example", "to show how", "to write to a file", "line by line."}
    for _, line := range linesToWrite {
        bytesWritten, err := writer.WriteString(line + "\n")
        if err != nil {
            log.Fatalf("Got error while writing to a file. Err: %s", err.Error())
        }
        fmt.Printf("Bytes Written: %d\n", bytesWritten)
        fmt.Printf("Available: %d\n", writer.Available())
        fmt.Printf("Buffered : %d\n", writer.Buffered())
    }
    writer.Flush()
}
```

**Output:**

```
Bytes Written: 19
Available: 4077
Buffered : 19
Bytes Written: 12
Available: 4065
Buffered : 31
Bytes Written: 19
Available: 4046
Buffered : 50
Bytes Written: 14
Available: 4032
Buffered : 64
```

## **Example: With Custom Buffer Size of 10 bytes**

We can also create a custom buffer size for our writer using bufio.NewWriterSize(). In the below example, we create a write with a custom buffer size of 10 bytes. If you carefully notice the bytes **“Available”** and **“Buffered”** in output, you will find that writes to file keep happening in between when the buffer is full and at the end when **writer.Flush()** is called.

```
package main
import (
    "bufio"
    "fmt"
    "log"
    "os"
)

//Write file using bufio writer
func main() {
    file, err := os.Create("./temp  .txt")
    if err != nil {
        log.Fatal(err)
    }
    writer := bufio.NewWriterSize(file, 10)
    linesToWrite := []string{"This is an example", "to show how", "to write to a file", "line by line."}
    for _, line := range linesToWrite {
        bytesWritten, err := writer.WriteString(line + "\n")
        if err != nil {
            log.Fatalf("Got error while writing to a file. Err: %s", err.Error())
        }
        fmt.Printf("Bytes Written: %d\n", bytesWritten)
        fmt.Printf("Available: %d\n", writer.Available())
        fmt.Printf("Buffered : %d\n", writer.Buffered())
    }
    writer.Flush()
}
```

**Output:**

```
Bytes Written: 19
Available: 1
Buffered : 9
Bytes Written: 12
Available: 9
Buffered : 1
Bytes Written: 19
Available: 0
Buffered : 10
Bytes Written: 14
Available: 6
Buffered : 4
```

# **Using file.Write()**

It does not maintain any buffer and writes to the file immediately as soon as the write is called. See the example below

```
package main

import (
    "log"
    "os"
)

func main() {
    file, err := os.Create("./test.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()
    linesToWrite := []string{"This is an example", "to show how", "to write to a file", "line by line."}
    for _, line := range linesToWrite {
        file.WriteString(line + "\n")
    }
}
```

# **Using ioutil.WriteFile()**

**ioutil.WriteFile()** is kind of a shortcut to writing to a file. It takes in three-parameter – (1) file (2) The string to be written (3) Permission mode of the file. The third parameter is used to create a file with that permission if the file doesn’t already exist. One calling **ioutil.WriteFile()**, it will

*   Create the file if not present with the specified permission
*   Write to the file
*   Close the file

See below example: If temp.txt doesn’t exist then it will create a new temp.txt file with permission 0777

```
package main

import (
    "io/ioutil"
    "log"
)

func main() {
    linesToWrite := "This is an example to show how to write to file using ioutil"
    err := ioutil.WriteFile("temp.txt", []byte(linesToWrite), 0777)
    if err != nil {
        log.Fatal(err)
    }
}
```

*   [buffered write](https://golangbyexample.com/tag/buffered-write/)*   [bufio](https://golangbyexample.com/tag/bufio/)*