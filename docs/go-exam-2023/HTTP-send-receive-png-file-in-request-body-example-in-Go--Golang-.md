<!--yml

类别：未分类

日期：2024-10-13 06:32:48

-->

# Go（Golang）中 HTTP 发送/接收 png 文件的请求体示例

> 来源：[`golangbyexample.com/http-png-post-golang/`](https://golangbyexample.com/http-png-post-golang/)

目录

**   概述

+   HTTP 服务器

+   HTTP 客户端*  *# **概述**

**multipart/form-data**内容类型可以用于在 HTTP POST 调用中发送 png 文件。表单数据将包含

+   png 文件名- 示例中的**test.png**，我们将在本教程中看到。

+   包含 png 文件内容的 key – 在教程示例中的**photo**

让我们来看一下 HTTP **客户端**和**服务器**的示例。

# **HTTP 服务器**

下面是相应的程序。

```go
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

让我们也理解一下程序。我们需要做的第一件事是调用**ParseMultipartForm**函数，传入**request**对象。

```go
request.ParseMultipartForm()
```

它将解析表单数据请求体。之后，我们可以在请求对象上调用**FormFile**函数，并传入 key 作为参数。它将返回给定 key 的**multipart.File**对象，在这里是**"photo"**。该对象是访问多部分消息中该 key 的文件部分的接口。程序使用它将文件保存到磁盘。

```go
_, err = io.Copy(tmpfile, file)
```

这就是 HTTP 服务器示例。运行服务器。它将监听**8080**端口。让我们创建一个 HTTP 客户端来测试上述服务器。下面是相关代码。

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

下面是相同的 HTTP 客户端示例代码。它在 HTTP 请求中向上面创建的服务器发送**multipart/form-data**请求体。

首先，我们必须创建一个多部分写入器 [`golang.org/pkg/mime/multipart/#Writer`](https://golang.org/pkg/mime/multipart/#Writer)

```go
writer := multipart.NewWriter(body)
```

多部分写入器提供了**CreateFormFile**方法，可以用来在多部分请求体中创建一个文件字段。**filename**是**test.png**，**key**名称是**"photo"**。

运行上述文件。它会将**test.png**发送到上面创建的服务器。请求完成后，你可以在服务器端检查。服务器将解析表单数据请求体，然后将文件内容保存为同名文件。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
