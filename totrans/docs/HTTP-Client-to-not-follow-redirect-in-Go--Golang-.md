<!--yml
category: 未分类
date: 2024-10-13 06:34:11
-->

# HTTP Client to not follow redirect in Go (Golang)

> 来源：[https://golangbyexample.com/http-no-redirect-client-golang/](https://golangbyexample.com/http-no-redirect-client-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [CheckDirect is nil](#CheckDirect_is_nil "CheckDirect is nil")
*   [CheckDirect is not-nil](#CheckDirect_is_not-nil "CheckDirect is not-nil")*  *# **Overview**

**http.Client** struct is used to make HTTP requests in golang.

[https://golang.org/src/net/http/client.go](https://golang.org/src/net/http/client.go)

**http.Client** lets you specify a policy of how redirects can be handled.  Below is the structure of **http.Client** struct

```
type Client struct {
	Transport RoundTripper
	CheckRedirect func(req *Request, via []*Request) error
	Jar CookieJar
	Timeout time.Duration
}
```

The **CheckRedirect** field is a function actually which lets you specify a way of how redirects can be handled. This function is called before following any future redirects. So basically whenever the server responds with a redirect the client will first call the **CheckRedirect** function to check if redirect should be followed or not. Below is the signature of the **CheckRedirect** function

```
func(req *Request, via []*Request) error
```

*   The first parameter **req** is the upcoming request

*   The second parameter **via** is the requests already made. That is why it is a slice. The first element is the oldest request.

There are two cases to consider

# **CheckDirect is nil**

You don’t specify the CheckRedirect field during the initialization of **http.Client** struct. In this case, default behavior kicks in, and the HTTP client will follow 10 redirects, and then it will return an error.  Below is the **defaultCheckRedirect** function which quits after 10 retries.

```
func defaultCheckRedirect(req *Request, via []*Request) error {
	if len(via) >= 10 {
		return errors.New("stopped after 10 redirects")
	}
	return nil
}
```

# **CheckDirect is not-nil**

You specify your own **CheckRedirect** function during the initialization of the **http.Client** struct. When you specify this function, then this function will be called before following any future redirect.  When you write your own CheckRedirect function you need to know the below things in mind
When the **CheckRedirect** function returns an error, then the HTTP client will return two things

*   The previous response along with body closed

*   The actual error (wrapped in a **url.Error)**

**There is also a special case when **CheckRedirect** function returns **ErrUseLastResponse** error. In this case, the client will return**

 ***   Most recent response along with body unclosed

*   Non-nil error

So basically when you don’t want HTTP Client to follow the redirect, then please specify your own **CheckRedirect** function to control the behavior the way you want.  For example below is one sample **CheckRedirect** function that will inform HTTP client to not follow any redirect and also not return any error, just return the last response along with body unclosed.

```
func MyCheckRedirect(req *Request, via []*Request) error {
	return http.ErrUseLastResponse
}
```

Here is the definition of **ErrUseLastResponse** in the http package

```
var ErrUseLastResponse = errors.New("net/http: use last response")
```

Create **http.Client** as below specifying the **CheckRedirect** function

```
client := &http.Client{
    CheckRedirect: func(req *Request, via []*Request) error {
	return http.ErrUseLastResponse
    },
}
```

Here is another sample **CheckRedirect** function which will only follow redirect two times and then return the error

```
func MyCheckRedirect(req *Request, via []*Request) error {
	if len(via) >= 2 {
		return errors.New("stopped after 10 redirects")
	}
	return nil
}
```

*   [redirect](https://golangbyexample.com/tag/redirect/)***