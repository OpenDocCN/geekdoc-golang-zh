<!--yml
category: 未分类
date: 2024-10-13 06:32:28
-->

# HTTP server or Parse incoming application/x-www-form-urlencoded request body in Go (Golang)

> 来源：[https://golangbyexample.com/url-encoded-body-golang/](https://golangbyexample.com/url-encoded-body-golang/)

**Note**: This post is parsing the **application/x-www-form-urlencoded** request at the server end. If you using an HTTP client and trying to send the **application/x-www-form-urlencoded** request then please see the below link

[https://golangbyexample.com/http-client-urlencoded-body-go](https://golangbyexample.com/http-client-urlencoded-body-go)/

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

The **applcation/x-www-form-urlencoded** content-type request body is like a giant query string. Similar to the query string in a URI it is a key-value pair having the below format

```
key1=value1&key2=value21&key2=value22&key3=value3
```

where there are below key-value pairs

*   key1, value1

*   key2 has two values i.e value21 and value22.

*   key3, value3

Each key-value pair is separated by **&** in case of multiple values for the same key there will be two entries of that key-value pair. Also, each key and value is URLencoded similar to the query string.

Now the question that might be coming to the mind is if **x-www-form-urlencoded** is just like query string then why does it exist. The reason is that the query string is part of the URI and since there is a limit on the length of the URI, you can send a limited number of key-value pairs in the query string.  While there is no limit for the length of the **x-www-form-urlencoded** request body. However, it is limited by the max request body size allowed by the server which is generally 10MB for most of the servers. Now let’s see how we can parse the **x-www-form-urlencoded** in golang

# **Example**

Request object of net/http package has two fields which can hold the form-data. [https://golang.org/src/net/http/request.go](https://golang.org/src/net/http/request.go)

The two fields are

*   **Form** – It holds combine data of query string and **x-www-form-urlencoded** request body

*   **PostForm**– It holds only **x-www-form-urlencoded** request body

To get the **x-www-form-urlencoded** request body we need to first call the below function on the request object

```
request.ParseForm()
```

This function essentially does below things

*   It parses the query string present in the URL and populates the **Form** field of the request object

```
request.Form
```

*   After that, if the request method is PUT, POST, or PATCH  and the request body content-type is x-www-form-urlencoded, then it also parses the request body and populates the above two fields of the request object

```
request.Form
request.PostForm
```

*   **request.PostForm** will be initialized to a non-nil, empty value if the request body content-type is not x-www-form-urlencoded or the request method is not PUT, POST, or PATCH

*   The body parameters will take precedence over URL query string values i.e if there is a key that ispresent in both form body and in query param. Then the value in the form body will take precedence

*   Also, note that this function is idempotent.

Both **Form** and **PostForm** field are of the below type

```
type Values map[string][]string
```

It is a map where

*   The key is a string

*   The value is an array of strings. It is an array of strings because there can be multiple values

So essentially to get the x-www-url-encoded request body we need to call the **ParseForm** function first on the request object. This will populate both the **Form** and **PostForm** field. Then we can access these fields to get the **x-www-form-urlencoded** body. For example, let’s say we have the below **x-www-form-urlencoded** body.

```
name=John
age=21
hobbies=sports
hobbies=music
```

Then you can access the **name** field like below

```
request.Form["name"]
request.PostForm["name"]
```

both will return

```
["John"]
```

It is an array because the key can have multiple values.

Also, the request’s object defines one more function **FormValue** which can be used to get the first value associated with the key. It just returns the first value and not the array. For example

```
request.FormValue("hobbies")
```

will return

```
sports
```

Let’s see a program for that as well

```
package main

import (
	"fmt"
	"net/http"
)

type employee struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}

func main() {
	createEmployeeHanlder := http.HandlerFunc(createEmployee)
	http.Handle("/employee", createEmployeeHanlder)
	http.ListenAndServe(":8080", nil)
}

func createEmployee(w http.ResponseWriter, r *http.Request) {
	headerContentTtype := r.Header.Get("Content-Type")
	if headerContentTtype != "application/x-www-form-urlencoded" {
		w.WriteHeader(http.StatusUnsupportedMediaType)
		return
	}
	r.ParseForm()
	fmt.Println("request.Form::")
	for key, value := range r.Form {
		fmt.Printf("Key:%s, Value:%s\n", key, value)
	}
	fmt.Println("\nrequest.PostForm::")
	for key, value := range r.PostForm {
		fmt.Printf("Key:%s, Value:%s\n", key, value)
	}

	fmt.Printf("\nName field in Form:%s\n", r.Form["name"])
	fmt.Printf("\nName field in PostForm:%s\n", r.PostForm["name"])
	fmt.Printf("\nHobbies field in FormValue:%s\n", r.FormValue("hobbies"))

	w.WriteHeader(200)
	return
}
```

In the above program, we are running a server on port **8080**. Also notice that we are first calling the **ParseForm** function and then accessing the **Form** and **PostForm** fields of the request object.

Also, see how we are accessing the value of the **name** field in **Form** as well as **PostForm**

```
r.Form["name"]
r.PostForm["name"]
```

Also, we are calling the **FormValue** function on the request object to access the **hobbies** field as below

```
r.FormValue("hobbies")
```

Run the above program. It will start a server at port **8080**

Now let’s try some curl calls.

*   Passing only the x-www-form-urlencoded body. No query string.

```
curl -v -X POST 'http://localhost:8080/employee' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'name=John' \
--data-urlencode 'age=18' \
--data-urlencode 'hobbies=sports' \
--data-urlencode 'hobbies=music'
```

**Output**

```
request.Form:: 
Key:name, Value:[John] 
Key:age, Value:[18] 
Key:hobbies, Value:[sports music] 

request.PostForm:: 
Key:name, Value:[John] 
Key:age, Value:[18] 
Key:hobbies, Value:[sports music] 

Name field in Form:[John] 

Name field in PostForm:[John] 

Hobbies field in FormValue:sports
```

*   Passing the x-www-form-urlencoded body. Passing an extra field **gender** in the query string. Notice in the output that the gender field is not present in **PostForm** but present in **Form**

```
curl -v -X POST 'http://localhost:8080/employee?gender=male' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'name=John' \
--data-urlencode 'age=18' \
--data-urlencode 'hobbies=sports' \
--data-urlencode 'hobbies=music'
```

**Output**

```
request.Form:: 
Key:age, Value:[18] 
Key:hobbies, Value:[sports music] 
Key:gender, Value:[male] 
Key:name, Value:[John] 

request.PostForm:: 
Key:name, Value:[John] 
Key:age, Value:[18] 
Key:hobbies, Value:[sports music] 

Name field in Form:[John] 

Name field in PostForm:[John] 

Hobbies field in FormValue:sports
```

*   Passing the x-www-form-urlencoded body. Also passing the same key ie **age** in the query string. **age=20** is passed in the query string and **age=18** is passed in the post body.  The value **20** for **age** will not be present in the **PostForm** field but present in the **Form** field. Since body parameters take precedence therefore **age=18** is the first value in the array in the **Form** field. If you run **r.FormValue(“age”)** then it will return 18 and not 20

```
curl -v -X POST 'http://localhost:8080/employee?age=20' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'name=John' \
--data-urlencode 'age=18' \
--data-urlencode 'hobbies=sports' \
--data-urlencode 'hobbies=music'
```

**Output**

```
request.Form:: 
Key:hobbies, Value:[sports music] 
Key:name, Value:[John] 
Key:age, Value:[18 20] 

request.PostForm:: 
Key:name, Value:[John] 
Key:age, Value:[18] 
Key:hobbies, Value:[sports music] 

Name field in Form:[John] 

Name field in PostForm:[John] 

Hobbies field in FormValue:sports
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*