<!--yml
category: 未分类
date: 2024-10-13 06:33:01
-->

# Why response body is closed in golang

> 来源：[https://golangbyexample.com/resposne-body-closed-golang/](https://golangbyexample.com/resposne-body-closed-golang/)

The response body should be closed after the response is fully read. This is done to prevent resource leak of connections. If the response body is not closed then the connection will not be released and hence it cannot be reused. From the official docs of **http.Client**

From the official docs of **http.Client**

[https://golang.org/pkg/net/http/#Client](https://golang.org/pkg/net/http/#Client)

If the Body is not both read to EOF and closed, the Client’s underlying RoundTripper (typically Transport) may not be able to re-use a persistent TCP connection to the server for a subsequent “keep-alive” request.

So essentially, transport may not reuse HTTP/1.x “keep-alive” TCP connections if the Body is not read to completion and closed.The **response.Body** implements the **io.ReadCloser** interface.

Also to mention that it is the caller’s responsibility to close the response body

Some important guidelines to follow 

*   Use the defer method to close the response body. This is done to make sure that the response body gets closed even in case of runtime error during reading and parsing of response.

*   Check for error first always. Only proceed to close the response body using defer if the error is nil. A nil error always indicates a non-nil response body. If the error is non-nil then handle that error and return. Also, note that closing a nil response body will cause a panic.

A simple example

```
resp, err := http.Get("http://google.com/")
if err != nil {
    //Handle the error here.
    return
}
defer resp.Body.Close()
//Read and parse response body here
```

Above is a simple example of how to handle HTTP error and when to close a response body. See how in the example we are closing the resp.Body using the defer function. And the response body is only closed if the error is nil

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)