<!--yml
category: 未分类
date: 2024-10-13 06:32:52
-->

# HTTP client/server multipart form-data request body example in Go (Golang)

> 来源：[https://golangbyexample.com/http-mutipart-form-body-golang/](https://golangbyexample.com/http-mutipart-form-body-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [HTTP Server Example](#HTTP_Server_Example "HTTP Server Example")
*   [HTTP Client Example](#HTTP_Client_Example "HTTP Client Example")*  *# **Overview**

In HTTP context, the **multipart/form-data** content-type is used for submitting HTML form. In the case of **multipart/form-data**, as the name suggests, the body consists of different parts separated by a delimiter or boundary where each part is described by its own headers. The delimiter or boundary is also sent as part of the header only.

When you sending an HTML form through a browser in an HTTP call, the data contents can be sent in as request body in the below two formats.

*   application/x-www-form-urlencoded
*   multipart/form-data

For most of the cases, **application/x-www-form-urlencoded** can be used. But if you need to upload files then **application/x-www-form-urlencoded** is not much efficient.

For example, let’s say that the below data needs to be sent.

*   name

*   age

Then **application/x-www-form-urlencoded** can be used to send the above data. But let’s say that you also need to send the profile photo of the user in the request as well. So the data is now as below

*   name

*   age

*   photo

In the above case, it will not be efficient to use **application/x-www-form-urlencoded** content-type. **multipart/form-data** should be used in this case. So for sending simple form data use **application/x-www-form-urlencoded** but if the form-data also needs to send binary data then use **multipart/form-data.**

Now let’s see an example of

*   HTTP Server for parsing multipart/form-data request

*   HTTP Client for sending multipart/form-data request

# **HTTP Server Example**

For HTTP server an incoming request is represented by the **request** struct

[https://golang.org/src/net/http/request.go](https://golang.org/src/net/http/request.go)

To parse the **multipart/form-data** request body we need to first call the below function on the **request** object

```
request.ParseMultipartForm()
```

What above function will do is that it will parse the incoming request body and the data will be loaded in the below fields of the **request** object

*   **MultipartForm –** The entire **multipart/form-data** request body will be loaded into this field. For example, in the above case, it will hold the **name**, **age** field as well as **photo** field. It is represented in the below format

```
type Form struct {
	Value map[string][]string
	File  map[string][]*FileHeader
}
```

It has two parts. The Value holds all the non-files data. So it will hold the **name** and **age** key data.  The **File** part holds all file data. So it will hold the data for **photo** key.  Both the parts have their value part as an array because for the same key there can be multiple values.

*   **Form** –  It holds combine data of query string and non-file fields of the **multipart/form-data** request body. For example, for the above case, it will only hold the name and age field. It has below format

```
map[string][]string
```

*   **PostForm –** It only holds the non-file fields of the **multipart/form-data** request body. So for the above case, it will only hold the **name** and **age** field

It also has the same format as **Form**

```
map[string][]string
```

Other than the above fields, the request object also provides the below utility function

*   **FormFile** –  This function can be used to get the first file for the given key. So this function will only return data for **photo** key and not for **name** and **age** key

*   **PostFormValue** – This function returns the first value for the given form key.  This function will only return data for **name** and **age** key and not for **photo** key

Let’s see different ways in which **name**, **key**, and **photo** values can be accessed

**Name Key**

```
request.Form["name"]
request.PostForm["name"] 
request.MultipartForm.Value["name"]
```

will return

```
["John"]
```

It returns an array because for the same key there can be different values.

While

```
request.PostFormValue("name") 
```

will return

```
John
```

It is not an array because as mentioned above this function returns the first value associated with the key.

**Age Key**

```
request.Form["age"]
request.PostForm["age"] 
request.MultipartForm.Value["age"]
```

will return

```
["21"]
```

While

```
request.PostFormValue("age") 
```

will return

```
21
```

**Photo Key**

```
request.MultipartForm.File["photo"]
```

will return the  **FileHeader** for the image. Bytes of the image can be accessed using that

```
[FileHeader_object_of_the_image]
```

While

```
request.FormFile("photo") 
```

will return

```
FileHeader_object_of_the_image
```

It is not an array because as mentioned above this function returns the first file associated with the key.

Now let’s see an example

```
package main
import (
	"fmt"
	"io"
	"mime/multipart"
	"net/http"
	"os"
)
func main() {
	createEmployeeHanlder := http.HandlerFunc(createEmployee)
	http.Handle("/employee", createEmployeeHanlder)
	http.ListenAndServe(":8080", nil)
}
func createEmployee(w http.ResponseWriter, request *http.Request) {
	err := request.ParseMultipartForm(32 << 20) // maxMemory 32MB
	if err != nil {
		w.WriteHeader(http.StatusBadRequest)
		return
	}
	//Access the name key - First Approach
	fmt.Println(request.Form["name"])
	//Access the name key - Second Approach
	fmt.Println(request.PostForm["name"])
	//Access the name key - Third Approach
	fmt.Println(request.MultipartForm.Value["name"])
	//Access the name key - Fourth Approach
	fmt.Println(request.PostFormValue("name"))
	//Access the age key - First Approach
	fmt.Println(request.Form["age"])
	//Access the age key - Second Approach
	fmt.Println(request.PostForm["age"])
	//Access the age key - Third Approach
	fmt.Println(request.MultipartForm.Value["age"])
	//Access the age key - Fourth Approach
	fmt.Println(request.PostFormValue("age"))
	//Access the photo key - First Approach
	_, h, err := request.FormFile("photo")
	if err != nil {
		w.WriteHeader(http.StatusBadRequest)
		return
	}
	saveFile(h, "formfile")
	//Access the photo key - Second Approach
	for _, h := range request.MultipartForm.File["photo"] {
		err := saveFile(h, "mapaccess")
		if err != nil {
			w.WriteHeader(http.StatusBadRequest)
			return
		}
	}
	w.WriteHeader(200)
	return
}
```

Notice in the above program how are we printing the **name** key using all the approaches

```
//Access the name key - First Approach
fmt.Println(request.Form["name"])
//Access the name key - Second Approach
fmt.Println(request.PostForm["name"])
//Access the name key - Third Approach
fmt.Println(request.MultipartForm.Value["name"])
//Access the name key - Fourth Approach
fmt.Println(request.PostFormValue("name"))
```

And similar for the **age** key

Then we are saving the file present in the request body first using **FormFile** method and then using **request.MultipartForm.File** map**.** Since we are saving the same file using the two methods, we send a prefix argument to the **saveFile** function so that it saves files with different names. This is done so that the file saved by **FormFile** is not overwritten by the file saved by **request.MultipartForm.File.** This is just for a demonstration that both methods can be used to extract file data from the request. **FormFile** method uses the prefix **formfile** while **request.MultipartForm.File** uses **mapaccess** prefix for saving the file

Now let's make some curl calls. Create a file named **test.png** in the same folder from which you will run the curl command

*   Sending **name**, **age** and **photo**

```
curl --location --request PUT 'http://localhost:8080/employee' \
--header 'Content-Type: multipart/form-data' \
--form 'name=John' \
--form 'age=23' \
--form 'photo=@test.png'
```

**Output**

You can notice from the output that it is printing exactly what we have discussed earlier.

```
[John] 
[John] 
[John] 
John 
[23] 
[23] 
[23] 
23
```

It will also save two files with the below names

```
formfile-test.png
mapaccess-test.png
```

Now let's see the second curl example

*   Sending **name**, **age,** and **photo** but in this example, the same key **'photo'** is used for sending two files.

```
curl --location --request PUT 'http://localhost:8080/employee' \
--header 'Content-Type: multipart/form-data' \
--form 'name=John' \
--form 'age=23' \
--form 'photo=@test1.png' \
--form 'photo=@test2.png'
```

**Output**

```
[John] 
[John] 
[John] 
John 
[23] 
[23] 
[23] 
23
```

This time three files will be saved with the below names. Since FormFile only accesses the first file present with a given key therefore only **formfile-test1.png** is saved by the **FormFile** method.

```
formfile-test1.png
mapaccess-test2.png
mapaccess-test2.png
```

Let's see one more curl example

*   Sending two values for **name** key

```
curl --location --request PUT 'http://localhost:8080/employee' \
--header 'Content-Type: multipart/form-data' \
--form 'name=John' \
--form 'name=John2' \
--form 'age=23' \
--form 'photo=@test.png'
```

**Output**

```
[John John2] 
[John John2] 
[John John2] 
John 
[23] 
[23] 
[23]
23
```

rqeuest.**PostFormValue** returns

```
John
```

while all other options return both the values

```
[John John2]
```

# **HTTP Client Example**

Below is the code for an example of an HTTP client for the same. It is sending the **multipart/form-data** request body in an HTTP request to the server created in the above example.

First, we have to create a multipart Writer [https://golang.org/pkg/mime/multipart/#Writer](https://golang.org/pkg/mime/multipart/#Writer)

```
writer := multipart.NewWriter(body)
```

The multipart writer provides two methods

*   **CreateFormField**  - Used to create a text field to be sent in the multipart request body. We will create the **name** and **age** field using this method

*   **CreateFormFile** - Used to create a file field to be sent in the multipart request body. We will create the **photo** field using this method

Now let's see the program

```
package main
import (
    "bytes"
    "io"
    "log"
    "mime/multipart"
    "net/http"
    "os"
    "strings"
    "time"
)
func main() {
    call("http://localhost:8080/employee", "POST")
}
func call(urlPath, method string) error {
    client := &http.Client{
        Timeout: time.Second * 10,
    }
    // New multipart writer.
    body := &bytes.Buffer{}
    writer := multipart.NewWriter(body)
    fw, err := writer.CreateFormField("name")
    if err != nil {
    }
    _, err = io.Copy(fw, strings.NewReader("John"))
    if err != nil {
        return err
    }
    fw, err = writer.CreateFormField("age")
    if err != nil {
    }
    _, err = io.Copy(fw, strings.NewReader("23"))
    if err != nil {
        return err
    }
    fw, err = writer.CreateFormFile("photo", "test.png")
    if err != nil {
    }
    file, err := os.Open("test.png")
    if err != nil {
        panic(err)
    }
    _, err = io.Copy(fw, file)
    if err != nil {
        return err
    }
    // Close multipart writer.
    writer.Close()
    req, err := http.NewRequest("POST", "http://localhost:8080/employee", bytes.NewReader(body.Bytes()))
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

Run this program and see the output at your server end as the above client makes a call to your server created in the first example. The server program will give the below output similar to the curl calls.

```
[John] 
[John] 
[John] 
John 
[23] 
[23] 
[23] 
23
```

Also, the server will save two files with the below names similar to the curl calls

```
formfile-test.png
mapaccess-test.png
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*