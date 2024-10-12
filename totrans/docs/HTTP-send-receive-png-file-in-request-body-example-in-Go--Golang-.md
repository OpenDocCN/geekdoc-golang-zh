<!--yml
category: 未分类
date: 2024-10-13 06:32:48
-->

# HTTP send/receive png file in request body example in Go (Golang)

> 来源：[https://golangbyexample.com/http-png-post-golang/](https://golangbyexample.com/http-png-post-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [HTTP Server](#HTTP_Server "HTTP Server")
*   [HTTP Client](#HTTP_Client "HTTP Client")*  *# **Overview**

**multipart/form-data** content-type can be used to send the png files in an HTTP POST call. The form-data will contain

*   png filename- **test.png** in the example that we will see in this tutorial
*   key that will contain the png file content – **photo** in the example in the tutorial

Let’s see an example of both HTTP **client** and **server**

# **HTTP Server**

Below is the program for the same.

```
package main

import (
	"io"
	"net/http"
	"os"
)

func main() {
	createImageHandler := http.HandlerFunc(createImage)
	http.Handle("/png", createImageHandler)
	http.ListenAndServe(":8080", nil)
}

func createImage(w http.ResponseWriter, request *http.Request) {
	err := request.ParseMultipartForm(32 << 20) // maxMemory 32MB
	if err != nil {
		w.WriteHeader(http.StatusBadRequest)
		return
	}
	//Access the photo key - First Approach
	file, h, err := request.FormFile("photo")
	if err != nil {
		w.WriteHeader(http.StatusBadRequest)
		return
	}
	tmpfile, err := os.Create("./" + h.Filename)
	defer tmpfile.Close()
	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		return
	}
	_, err = io.Copy(tmpfile, file)
	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		return
	}

	w.WriteHeader(200)
	return
}
```

Let's understand the program also. The first thing we need to do is to call **ParseMultipartForm** function on the **request** object

```
request.ParseMultipartForm()
```

It will parse the form data request body. After that, we can call the **FormFile** function on the request object passing the key as an argument. It will return the **multipart.File** object for the given key which is **"photo"** here. This object is an interface to access the file part of a multipart message for that key. The program uses it to save the file to the disk.

```
_, err = io.Copy(tmpfile, file)
```

This was the HTTP server example. Run the server. It will listen on **8080** port. Let's create an HTTP client to test the above server as well. Below is the code for that.

# **HTTP Client**

```
package main

import (
	"bytes"
	"io"
	"log"
	"mime/multipart"
	"net/http"
	"os"
	"time"
)

func main() {
	call("http://localhost:8080/png", "POST")
}

func call(urlPath, method string) error {
	client := &http.Client{
		Timeout: time.Second * 10,
	}

	// New multipart writer.
	body := &bytes.Buffer{}
	writer := multipart.NewWriter(body)
	fw, err := writer.CreateFormFile("photo", "test.png")
	if err != nil {
		return err
	}
	file, err := os.Open("test.png")
	if err != nil {
		return err
	}
	_, err = io.Copy(fw, file)
	if err != nil {
		return err
	}
	writer.Close()
	req, err := http.NewRequest(method, urlPath, bytes.NewReader(body.Bytes()))
	if err != nil {
		return err
	}
	req.Header.Set("Content-Type", writer.FormDataContentType())
	rsp, _ := client.Do(req)
	if rsp.StatusCode != http.StatusOK {
		log.Printf("Request failed with response code: %d", rsp.StatusCode)
	}
	return nil
}
```

Below is the code for an example of an HTTP client for the same. It is sending the **multipart/form-data** request body in an HTTP request to the server created in the above example.

First, we have to create a multipart Writer [https://golang.org/pkg/mime/multipart/#Writer](https://golang.org/pkg/mime/multipart/#Writer)

```
writer := multipart.NewWriter(body)
```

The multipart writer provides the **CreateFormFile** method which can be used to create a file field to be sent in the multipart request body. The **filename** is **test.png** and the **key** name is **"photo".**

Run the above file. It will send the **test.png** to the server created above. You can check at the server end after the request finishes. The server will parse the form-data request body and then save the file contents to a file with the same name.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*