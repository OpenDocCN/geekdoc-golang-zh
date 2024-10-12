<!--yml
category: 未分类
date: 2024-10-13 06:09:16
-->

# Download an image or file from a URL in Go (Golang)

> 来源：[https://golangbyexample.com/download-image-file-url-golang/](https://golangbyexample.com/download-image-file-url-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# Overview

Below are the steps to download an image or a file from a URL

*   use **http.Get(URL)** function to get the bytes of the file from the URL.

*   Create an empty file using **os.Create()** function

*   Use **io.Copy()** function to copy the downloaded bytes to the file created in step 2

# Code

```
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

**Output:**

```
File sample.pdf downlaod in current working directory
```

*   [golang](https://golangbyexample.com/tag/golang/)*   [pdf](https://golangbyexample.com/tag/pdf/)*