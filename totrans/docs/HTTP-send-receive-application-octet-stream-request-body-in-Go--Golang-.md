<!--yml
category: 未分类
date: 2024-10-13 06:32:56
-->

# HTTP send/receive application octet-stream request body in Go (Golang)

> 来源：[https://golangbyexample.com/octet-stream-http-golang/](https://golangbyexample.com/octet-stream-http-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [HTTP Client Example](#HTTP_Client_Example "HTTP Client Example")
*   [HTTP Server Example](#HTTP_Server_Example "HTTP Server Example")*  *# **Overview**

**application/octet-stream** content-type is used to transfer binary data in the HTTP request body. So **application/octet-stream** is used for sending files over HTTP request. Let’s see an example of both HTTP client and server in golang for sending and receiving **application/octet-stream** data.

Let’s create two folders first.

*   client

*   server

Also, create a file named **photo.png** in the **client** folder

# **HTTP Client Example**

Below is the program for the same

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

In the above program, we are sending the file **photo.png** as **application/octet-stream** request body in the POST request. photo.png is first converted into bytes and these bytes are sent as **application/octet-stream** body. For that, bytes of the file are read first

```
b, err := ioutil.ReadFile("photo.png")
```

After the file is converted into bytes, it is passed to the **http.NewRequest** method as below for the third argument

```
bytes.NewReader(b)
```

The **bytes.NewReader** returns the **bytes.Reader** instance. **bytes.Reader** implements both **io.Reader** and **io.Writer.** The **http.NewRequest** method accepts the **io.Reader** for the body part. Therefore **bytes.Reader** instance can be passed as the third argument to http.NewRequest

```
req, err := http.NewRequest(method, urlPath, bytes.NewReader(b))
```

Also in the above program, we are making a call to the below API

```
http://localhost:8080/photo
```

Let’s create a server that will listen on port **8080** so that we can test the above client as well. Also, the server will parse the application/octet-stream body and save it as **photo.png**. Let’s see the program for the server now

# **HTTP Server Example**

Below is the program for the same.

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

In the above program, we have created an API that will listen on port 8080\. The API signature will be

```
http://localhost:8080/photo
```

It reads the bytes from the POST body and saves them to a file **photo.png**

Let’s run both the Server and the Client. Run the server first.

```
go run server/main.go
```

It will start listening to port 8080\. After that run the client.

```
go run client/main.go
```

After the API is successfully executed, check at the server end. A file named **photo.png** will be created at the server end.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*