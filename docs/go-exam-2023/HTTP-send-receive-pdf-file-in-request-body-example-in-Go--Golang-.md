<!--yml

分类：未分类

日期：2024-10-13 06:32:43

-->

# 在 Go（Golang）中请求体中发送/接收 pdf 文件的示例

> 来源：[`golangbyexample.com/http-pdf-post-go/`](https://golangbyexample.com/http-pdf-post-go/)

目录

+   概述

+   HTTP 服务器

+   HTTP 客户端

# **概述**

**multipart/form-data**内容类型可用于在 HTTP POST 调用中发送 pdf 文件。表单数据将包含

+   pdf 文件名 - 本教程中将看到的**test.pdf**

+   将包含 pdf 文件内容的键 - 本教程示例中的**photo**

让我们看看 HTTP **客户端**和**服务器**的示例

# **HTTP 服务器**

以下是相应的程序。

```go
package main

import (
	"io"
	"net/http"
	"os"
)

func main() {
	createImageHandler := http.HandlerFunc(createImage)
	http.Handle("/pdf", createImageHandler)
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

让我们也理解程序。我们需要做的第一件事是在**请求**对象上调用**ParseMultipartForm**函数

```go
request.ParseMultipartForm()
```

它将解析表单数据请求体。之后，我们可以在请求对象上调用**FormFile**函数，传入键作为参数。它将返回给定键的**multipart.File**对象，这里的键是**"photo"**。该对象是访问该键的多部分消息文件部分的接口。程序使用它将文件保存到磁盘。

```go
_, err = io.Copy(tmpfile, file)
```

这是 HTTP 服务器示例。运行服务器。它将在**8080**端口监听。让我们创建一个 HTTP 客户端来测试上述服务器。以下是相关代码。

# **HTTP 客户端**

```go
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
	call("http://localhost:8080/pdf", "POST")
}

func call(urlPath, method string) error {
	client := &http.Client{
		Timeout: time.Second * 10,
	}

	// New multipart writer.
	body := &bytes.Buffer{}
	writer := multipart.NewWriter(body)
	fw, err := writer.CreateFormFile("photo", "test.pdf")
	if err != nil {
		return err
	}
	file, err := os.Open("test.pdf")
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

以下是一个 HTTP 客户端的示例代码。它在 HTTP 请求中向上面创建的服务器发送**multipart/form-data**请求体。

首先，我们必须创建一个多部分写入器 [`golang.org/pkg/mime/multipart/#Writer`](https://golang.org/pkg/mime/multipart/#Writer)

```go
writer := multipart.NewWriter(body)
```

多部分写入器提供了**CreateFormFile**方法，可以用来创建一个在多部分请求体中发送的文件字段。**文件名**是**test.pdf**，**键**名是**"photo"**。

运行上述文件。它将把**test.pdf**发送到上面创建的服务器。请求完成后，你可以在服务器端检查。服务器会解析表单数据请求体，然后将文件内容保存到同名文件中。


