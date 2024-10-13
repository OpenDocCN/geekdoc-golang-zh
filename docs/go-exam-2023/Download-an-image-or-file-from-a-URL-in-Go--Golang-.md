<!--yml

分类：未分类

日期：2024-10-13 06:09:16

-->

# 在 Go (Golang) 中从 URL 下载图像或文件。

> 来源：[`golangbyexample.com/download-image-file-url-golang/`](https://golangbyexample.com/download-image-file-url-golang/)

目录

**   概述

+   代码*  *# 概述

以下是从 URL 下载图像或文件的步骤。

+   使用 **http.Get(URL)** 函数从 URL 获取文件的字节。

+   使用 **os.Create()** 函数创建一个空文件。

+   使用 **io.Copy()** 函数将下载的字节复制到第 2 步中创建的文件中。

# 代码

```go
package main

import (
	"errors"
	"fmt"
	"io"
	"log"
	"net/http"
	"os"
)

func main() {
	fileName := "sample.pdf"
	URL := "http://www.africau.edu/images/default/sample.pdf"
	err := downloadFile(URL, fileName)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("File %s downlaod in current working directory", fileName)
}

func downloadFile(URL, fileName string) error {
	//Get the response bytes from the url
	response, err := http.Get(URL)
	if err != nil {
		return err
	}
	defer response.Body.Close()

	if response.StatusCode != 200 {
		return errors.New("Received non 200 response code")
	}
	//Create a empty file
	file, err := os.Create(fileName)
	if err != nil {
		return err
	}
	defer file.Close()

	//Write the bytes to the fiel
	_, err = io.Copy(file, response.Body)
	if err != nil {
		return err
	}

	return nil
} 
```

**输出：**

```go
File sample.pdf downlaod in current working directory
```

+   [golang](https://golangbyexample.com/tag/golang/)*   [pdf](https://golangbyexample.com/tag/pdf/)*
