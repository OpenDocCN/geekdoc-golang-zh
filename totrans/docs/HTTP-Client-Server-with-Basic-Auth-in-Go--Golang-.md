<!--yml
category: 未分类
date: 2024-10-13 06:32:18
-->

# HTTP Client/Server with Basic Auth in Go (Golang)

> 来源：[https://golangbyexample.com/http-basic-auth-golang/](https://golangbyexample.com/http-basic-auth-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [HTTP Server Basic Auth Example](#HTTP_Server_Basic_Auth_Example "HTTP Server Basic Auth Example")
*   [HTTP Client Basic Auth Example](#HTTP_Client_Basic_Auth_Example "HTTP Client Basic Auth Example")*  *# **Overview**

Basic auth is the simplest form of providing access controls for resources on web server.  Basic Access Authentication is a way of providing user name and password to the server while making an HTTP request.  The credentials are send in the headers of the request. Below is the header and format in which credentials are send.

```
Authorization : Basic <credentials></credentials>
```

*   Header name is **“Authorization”**

*   credentials are send Base64 encoding of username and password joined by a colon(:). Basically Base64 encoding of below string

```
<username>:<password></password></username>
```

*   credentials is prefixed with **Basic**

Basic Auth is specified in RFC 7617

[https://tools.ietf.org/html/rfc7617](https://tools.ietf.org/html/rfc7617)

Basic auth doesn’t require any kind of sessions identifiers or cookies. Also since the credentials is send as bas64 encoding only so there is no encryption involved. Hence basic auth is used only with HTTPS for security reasons.

# **HTTP Server Basic Auth Example**

Let’s first see basic auth wrt to **HTTP** server. **net/http** package of golang provides a method which is defined on the ***http.Request** struct which returns the username and password which is present in the incoming request’s **Authorization** Header. Below is the signature of the method

```
func (r *Request) BasicAuth() (username, password string, ok bool)
```

What this method does it that it checks the **Authorization** header and then extracts the username and password from  Base64 encoded value and return it. If there is any issue in parsing it will return **ok** variable as false. So while using this function, we first need to check the value of **ok** variable. If the **ok** variable is true then we can further match the username and password and verify if it is correct.

Let’s see a program for this

```
package main

import (
	"fmt"
	"net/http"
)

var (
	username = "abc"
	password = "123"
)

func main() {
	handler := http.HandlerFunc(handleRequest)
	http.Handle("/example", handler)
	http.ListenAndServe(":8080", nil)
}

func handleRequest(w http.ResponseWriter, r *http.Request) {

	u, p, ok := r.BasicAuth()
	if !ok {
		fmt.Println("Error parsing basic auth")
		w.WriteHeader(401)
		return
	}
	if u != username {
		fmt.Printf("Username provided is correct: %s\n", u)
		w.WriteHeader(401)
		return
	}
	if p != password {
		fmt.Printf("Password provided is correct: %s\n", u)
		w.WriteHeader(401)
		return
	}
	fmt.Printf("Username: %s\n", u)
	fmt.Printf("Password: %s\n", p)
	w.WriteHeader(200)
	return
}
```

Server accepts the below username and password

*   username is abc

*   password is 123

For making the request we have to do Base64 encoding of  below string which is a username and password joined by a single colon(:)

```
abc:123
```

Base64 encoding of above string will be.

```
YWJjOjEyMw==
```

You can check it here – [https://www.base64encode.org/](https://www.base64encode.org/)

Now make the below request

```
curl -v -X POST http://localhost:8080/example -H "Authorization: Basic YWJjOjEyMw=="
```

This request will get the status code as 200\. Also it will correctly print the username and password in the logs as well.

```
Username: abc
Password: 123
```

Now send the malformed **Base64** encoded value.

```
curl -v -X POST http://localhost:8080/example -H "Authorization: Basic YWJjOjEy"
```

It will get the status code as 401\. It will also print below in the logs

```
Error parsing basic auth
```

# **HTTP Client Basic Auth Example**

**net/http** package of golang also provides a method which is defined on the ***http.Request** struct which can be used to set the basic auth header.  Below is the signature of the method

```
func (r *Request) SetBasicAuth(username, password string)
```

What this method does it takes in **username** and **password** and sets Authorization header with base64 encoded value of username and password joined by a single colon(:).

Let’s see a program for this

```
package main

import (
	"fmt"
	"net/http"
	"time"
)

var (
	username = "abc"
	password = "123"
)

func main() {
	call("https://localhost:8080/example", "POST")
}

func call(url, method string) error {
	client := &http.Client{
		Timeout: time.Second * 10,
	}
	req, err := http.NewRequest(method, url, nil)
	if err != nil {
		return fmt.Errorf("Got error %s", err.Error())
	}
	req.SetBasicAuth(username, password)
	response, err := client.Do(req)
	if err != nil {
		return fmt.Errorf("Got error %s", err.Error())
	}
	defer response.Body.Close()
	return nil
}
```

In the above program we are making a call to the server which we had set up earlier. See how we set up the basic auth in the outgoing request

```
req.SetBasicAuth(username, password)
```

If you will run the above program you will get a 200 from the server which we had set up above.

**Note:** We are printing  **username** and **password** above just for illustration purposes. In real program they need to be kept encrypted in some secure manner.  Also we are sending a http request below and not https for basic auth. Again this was just for illustrated only and both client and server lie on the same machine. This is not recommended and basic auth should be used only with **HTTPS**

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*