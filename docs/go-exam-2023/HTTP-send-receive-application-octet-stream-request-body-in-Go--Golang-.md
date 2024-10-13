<!--yml

分类：未分类

日期：2024-10-13 06:32:56

-->

# 在Go（Golang）中发送/接收`application/octet-stream`请求主体。

> 来源：[https://golangbyexample.com/octet-stream-http-golang/](https://golangbyexample.com/octet-stream-http-golang/)

目录

**   [概述](#Overview "概述")

+   [HTTP 客户端示例](#HTTP_Client_Example "HTTP 客户端示例")

+   [HTTP 服务器示例](#HTTP_Server_Example "HTTP 服务器示例")*  *# **概述**

**application/octet-stream**内容类型用于在HTTP请求主体中传输二进制数据。因此，**application/octet-stream**用于通过HTTP请求发送文件。让我们看一个golang中HTTP客户端和服务器的示例，用于发送和接收**application/octet-stream**数据。

让我们首先创建两个文件夹。

+   客户端

+   服务器

同时在**client**文件夹中创建一个名为**photo.png**的文件。

# **HTTP 客户端示例**

下面是相应的程序。

**client/main.go**

```
package main

import (
	"bytes"
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"time"
)

func main() {
	err := call("http://localhost:8080/photo", "POST")
	if err != nil {
		fmt.Printf("Error occurred. Err: %s", err.Error())
	}
}

func call(urlPath, method string) error {
	client := &http.Client{
		Timeout: time.Second * 10,
	}

	b, err := ioutil.ReadFile("photo.png")
	if err != nil {
		return err
	}

	req, err := http.NewRequest(method, urlPath, bytes.NewReader(b))
	if err != nil {
		return err
	}
	req.Header.Set("Content-Type", "application/octet-stream")
	rsp, _ := client.Do(req)
	if rsp.StatusCode != http.StatusOK {
		log.Printf("Request failed with response code: %d", rsp.StatusCode)
	}
	return nil
}
```

在上述程序中，我们将文件**photo.png**作为**application/octet-stream**请求主体发送在POST请求中。首先将photo.png转换为字节，这些字节作为**application/octet-stream**主体发送。为此，首先读取文件的字节。

```
b, err := ioutil.ReadFile("photo.png")
```

文件转换为字节后，它将作为第三个参数传递给**http.NewRequest**方法，如下所示。

```
bytes.NewReader(b)
```

**bytes.NewReader**返回**bytes.Reader**实例。**bytes.Reader**实现了**io.Reader**和**io.Writer**。**http.NewRequest**方法接受`io.Reader`作为主体部分。因此，**bytes.Reader**实例可以作为第三个参数传递给http.NewRequest。

```
req, err := http.NewRequest(method, urlPath, bytes.NewReader(b))
```

在上述程序中，我们还调用了以下API。

```
http://localhost:8080/photo
```

让我们创建一个在端口**8080**上监听的服务器，以便我们也可以测试上述客户端。此外，服务器将解析`application/octet-stream`主体并将其保存为**photo.png**。现在让我们看看服务器的程序。

# **HTTP 服务器示例**

下面是相应的程序。

**server/main.go**

```
package main

import (
	"io/ioutil"
	"net/http"
	"os"
)

func main() {
	createEmployeeHanlder := http.HandlerFunc(createEmployee)
	http.Handle("/photo", createEmployeeHanlder)
	http.ListenAndServe(":8080", nil)
}

func createEmployee(w http.ResponseWriter, request *http.Request) {
	d, err := ioutil.ReadAll(request.Body)
	if err != nil {
		http.Error(w, err.Error(), http.StatusBadRequest)
	}
	tmpfile, err := os.Create("./" + "photo.png")
	defer tmpfile.Close()
	if err != nil {
		http.Error(w, err.Error(), http.StatusBadRequest)
	}
	tmpfile.Write(d)

	w.WriteHeader(200)
	return
}
```

在上述程序中，我们创建了一个将在端口8080上监听的API。API签名将是：

```
http://localhost:8080/photo
```

它从POST主体读取字节并将其保存到文件**photo.png**中。

让我们运行服务器和客户端。先运行服务器。

```
go run server/main.go
```

它将开始监听端口8080。之后运行客户端。

```
go run client/main.go
```

API成功执行后，请检查服务器端。服务器端将创建一个名为**photo.png**的文件。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
