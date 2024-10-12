<!--yml
category: 未分类
date: 2024-10-13 06:35:41
-->

# Read cookie http in Go (Golang)

> 来源：[https://golangbyexample.com/read-cookie-http-golang/](https://golangbyexample.com/read-cookie-http-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

net/http Request struct provides a convenient method to read a particular cookie given its name. Below is the signature of that method. [https://golang.org/pkg/net/http/#Request.Cookie](https://golang.org/pkg/net/http/#Request.Cookie)

```
func (r *Request) Cookie(name string) (*Cookie, error)
```

To print all cookies,  we can iterate over the **Cookies** method of **http.Request** struct. We can use a range keyword for that.

```
for _, c := range r.Cookies() {
     fmt.Println(c)
}
```

Both these functions will return the instance of the **Cookie** struct. A cookie in golang is represented as below in golang.

[https://golang.org/src/net/http/cookie.go](https://golang.org/src/net/http/cookie.go)

```
type Cookie struct {
	Name  string
	Value string

	Path       string    // optional
	Domain     string    // optional
	Expires    time.Time // optional
	RawExpires string    // for reading cookies only

	// MaxAge=0 means no 'Max-Age' attribute specified.
	// MaxAge<0 means delete cookie now, equivalently 'Max-Age: 0'
	// MaxAge>0 means Max-Age attribute present and given in seconds
	MaxAge   int
	Secure   bool
	HttpOnly bool
	SameSite SameSite
	Raw      string
	Unparsed []string // Raw text of unparsed attribute-value pairs
}
```

See [https://tools.ietf.org/html/rfc6265](https://tools.ietf.org/html/rfc6265) for details of each of the fields of the above cookie.

## **Program**

Below is the program for the same to illustrate the **Cookie** and **Cookies** method of the **http.Request** struct

```
package main

import (
	"fmt"
	"log"
	"net/http"
)

func main() {
	docHandler := http.HandlerFunc(docHandler)
	http.Handle("/doc", docHandler)

	http.ListenAndServe(":8080", nil)
}

func docHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Println("Cookies in API Call:")

	tokenCookie, err := r.Cookie("token")
	if err != nil {
		log.Fatalf("Error occured while reading cookie")
	}
	fmt.Println("\nPrinting cookie with name as token")
	fmt.Println(tokenCookie)

	fmt.Println("\nPrinting all cookies")
	for _, c := range r.Cookies() {
		fmt.Println(c)
	}
	fmt.Println()
	w.WriteHeader(200)
	w.Write([]byte("Doc Get Successful"))
	return
}
```

Run the program above and make the below curl call

```
curl -X GET localhost:8080/doc --cookie "id=abcd; token=some_token"
```

The curl call is passing two cookie name-value pairs

*   id=abcd

*   token=some_token

It will give below output

```
Cookies in API Call:

Printing cookie with name as "token"
token=some_token

Printing all cookies
id=abcd
token=some_token
```

This is how we print a particular cookie with a given name **“token”**

```
tokenCookie, err := r.Cookie("token")
```

It prints as seen from the output

```
token=some_token
```

This is how we print all the cookies

```
for _, c := range r.Cookies() {
     fmt.Println(c)
}
```

It outputs both the cookies name-value pairs that we had sent in the curl call

```
id=abcd
token=some_token
```

That was all about cookies in golang. Hope you have liked the tutorial. Please share feedback in the comments.

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [cookie](https://golangbyexample.com/tag/cookie/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*