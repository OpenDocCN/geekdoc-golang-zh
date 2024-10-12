<!--yml
category: 未分类
date: 2024-10-13 06:39:30
-->

# Read a file into a variable in Go (Golang)

> 来源：[https://golangbyexample.com/read-file-variable-golang/](https://golangbyexample.com/read-file-variable-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Using ReadFile function provided by the ioutil package](#Using_ReadFile_function_provided_by_the_ioutil_package "Using ReadFile function provided by the ioutil package ")
*   [Using os.Open and then using bytes.Buffer](#Using_osOpen_and_then_using_bytesBuffer "Using os.Open and then using bytes.Buffer")
*   [Using os.Open and then using strings.Builder](#Using_osOpen_and_then_using_stringsBuilder "Using os.Open and then using strings.Builder")*  *## **Overview**

There are various ways of reading a file into a variable in golang. Here are some of the ways

*   Using **ReadFile** function provided by the **ioutil** package

*   Using **os.Open** and then using **bytes.Buffer**

*   Using **os.Open** and then using **strings.Builder**

.Also, note reading the entire file into a variable only makes sense if you are reading a small file. If the file is large then it makes sense to read it line by line. Refer to this article for the same.

[https://golangbyexample.com/read-large-file-line-by-line-go/](https://golangbyexample.com/read-large-file-line-by-line-go/)

Note: Before trying out examples in this tutorial, please create a file named **test.png** at the location from which you will be running the program

## **Using ReadFile function provided by the ioutil package**

[https://golang.org/pkg/io/ioutil/#ReadFile](https://golang.org/pkg/io/ioutil/#ReadFile)

Below is the program for the same

```
package main

import (
    "fmt"
    "io/ioutil"
)

func main() {
    fileBytes, err := ioutil.ReadFile("test.png")
    if err != nil {
        panic(err)
    }
    fileString := string(fileBytes)
    fmt.Println(fileString)
}
```

**Output**

```
Some Garbage Output depending upon the file
```

## **Using os.Open and then using bytes.Buffer**

[https://golang.org/pkg/bytes/#Buffer](https://golang.org/pkg/bytes/#Buffer)

Below is the program for the same

```
package main

import (
	"bytes"
	"fmt"
	"log"
	"os"
)

func main() {
	file, err := os.Open("test.png")
	if err != nil {
		log.Fatalf("Error while opening file. Err: %s", err)
	}
	defer file.Close()

	fileBuffer := new(bytes.Buffer)
	fileBuffer.ReadFrom(file)
	fileString := fileBuffer.String()

	fmt.Print(fileString)
}
```

**Output**

```
Some Garbage Output depending upon the file
```

## Using **os.Open** and then using **strings.Builder**

[https://golang.org/pkg/strings/#Builder](https://golang.org/pkg/strings/#Builder)

Below is the program for the same

```
package main

import (
	"bytes"
	"fmt"
	"log"
	"os"
)

func main() {
	file, err := os.Open("test.png")
	if err != nil {
		log.Fatalf("Error while opening file. Err: %s", err)
	}
	defer file.Close()

	fileBuffer := new(bytes.Buffer)
	fileBuffer.ReadFrom(file)
	fileString := fileBuffer.String()

	fmt.Print(fileString)
}
```

**Output**

```
Some Garbage Output depending upon the file
```

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*