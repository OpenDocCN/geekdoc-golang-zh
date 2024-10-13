<!--yml

类别：未分类

日期：2024-10-13 06:32:52

-->

# Go语言中的HTTP客户端/服务器multipart表单数据请求体示例

> 来源：[https://golangbyexample.com/http-mutipart-form-body-golang/](https://golangbyexample.com/http-mutipart-form-body-golang/)

目录

**   [概览](#Overview "Overview")

+   [HTTP 服务器示例](#HTTP_Server_Example "HTTP Server Example")

+   [HTTP 客户端示例](#HTTP_Client_Example "HTTP Client Example")*  *# **概览**

在HTTP上下文中，**multipart/form-data**内容类型用于提交HTML表单。在**multipart/form-data**的情况下，正如名称所示，主体由不同部分组成，这些部分由分隔符或边界分开，每个部分都有自己的头部描述。分隔符或边界也作为头部的一部分发送。

当你通过浏览器在HTTP调用中发送HTML表单时，数据内容可以通过以下两种格式作为请求体发送。

+   application/x-www-form-urlencoded

+   multipart/form-data

在大多数情况下，可以使用**application/x-www-form-urlencoded**。但如果需要上传文件，则**application/x-www-form-urlencoded**就不太高效。

例如，假设需要发送以下数据。

+   姓名

+   年龄

然后可以使用**application/x-www-form-urlencoded**发送上述数据。但假设你还需要在请求中发送用户的头像。所以现在的数据如下

+   姓名

+   年龄

+   照片

在上述情况下，使用**application/x-www-form-urlencoded**内容类型将不高效。在这种情况下，应使用**multipart/form-data**。因此，发送简单表单数据时使用**application/x-www-form-urlencoded**，但如果表单数据还需要发送二进制数据，则使用**multipart/form-data**。

现在让我们看一个例子

+   解析multipart/form-data请求的HTTP服务器

+   用于发送multipart/form-data请求的HTTP客户端

# **HTTP 服务器示例**

对于HTTP服务器，传入的请求由**请求**结构表示

[https://golang.org/src/net/http/request.go](https://golang.org/src/net/http/request.go)

要解析**multipart/form-data**请求体，我们需要先在**请求**对象上调用以下函数

```
request.ParseMultipartForm()
```

上述函数的作用是解析传入的请求体，并将数据加载到**请求**对象的以下字段中

+   **MultipartForm –** 整个**multipart/form-data**请求体将加载到此字段中。例如，在上述情况下，它将保存**姓名**、**年龄**字段以及**照片**字段。其表示格式如下

```
type Form struct {
	Value map[string][]string
	File  map[string][]*FileHeader
}
```

它有两个部分。Value保存所有非文件数据。因此，它将保存**姓名**和**年龄**的键数据。**文件**部分保存所有文件数据。因此，它将保存**照片**键的数据。这两个部分的值部分都是数组，因为对于同一个键可以有多个值。

+   **表单** – 它包含**multipart/form-data**请求体的查询字符串和非文件字段的组合数据。例如，在上述情况下，它只会包含名称和年龄字段。它的格式如下：

```
map[string][]string
```

+   **PostForm –** 它只包含**multipart/form-data**请求体的非文件字段。因此在上述情况下，它只会包含**名称**和**年龄**字段。

它的格式也与**表单**相同。

```
map[string][]string
```

除上述字段外，请求对象还提供以下实用函数。

+   **FormFile** – 此函数可用于获取给定键的第一个文件。因此，该函数只会返回**照片**键的数据，而不返回**名称**和**年龄**键的数据。

+   **PostFormValue** – 此函数返回给定表单键的第一个值。该函数只会返回**名称**和**年龄**键的数据，而不返回**照片**键的数据。

让我们看看可以以不同方式访问**名称**、**键**和**照片**值。

**名称键**

```
request.Form["name"]
request.PostForm["name"] 
request.MultipartForm.Value["name"]
```

将返回

```
["John"]
```

它返回一个数组，因为对于同一个键可以有不同的值。

当

```
request.PostFormValue("name") 
```

将返回

```
John
```

它不是一个数组，因为如上所述，此函数返回与键关联的第一个值。

**年龄键**

```
request.Form["age"]
request.PostForm["age"] 
request.MultipartForm.Value["age"]
```

将返回

```
["21"]
```

当

```
request.PostFormValue("age") 
```

将返回

```
21
```

**照片键**

```
request.MultipartForm.File["photo"]
```

将返回该图像的**FileHeader**。图像的字节可以通过它访问。

```
[FileHeader_object_of_the_image]
```

当

```
request.FormFile("photo") 
```

将返回

```
FileHeader_object_of_the_image
```

它不是一个数组，因为如上所述，此函数返回与键关联的第一个文件。

现在让我们看看一个示例。

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

注意在上述程序中，我们是如何使用所有方法打印**名称**键的。

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

对于**年龄**键也是类似的。

然后我们使用**FormFile**方法首先保存请求体中的文件，然后使用**request.MultipartForm.File**映射进行保存。由于我们使用这两种方法保存同一个文件，因此我们向**saveFile**函数发送一个前缀参数，以便它以不同的名称保存文件。这是为了确保**FormFile**保存的文件不会被**request.MultipartForm.File**保存的文件覆盖。这只是为了演示这两种方法都可以用于从请求中提取文件数据。**FormFile**方法使用前缀**formfile**，而**request.MultipartForm.File**使用**mapaccess**前缀来保存文件。

现在让我们进行一些curl调用。在运行curl命令的同一文件夹中创建一个名为**test.png**的文件。

+   发送**名称**、**年龄**和**照片**。

```
curl --location --request PUT 'http://localhost:8080/employee' \
--header 'Content-Type: multipart/form-data' \
--form 'name=John' \
--form 'age=23' \
--form 'photo=@test.png'
```

**输出**

从输出中可以注意到，它正好打印了我们之前讨论的内容。

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

它还会保存两个文件，文件名如下：

```
formfile-test.png
mapaccess-test.png
```

现在让我们看看第二个curl示例。

+   发送**名称**、**年龄**和**照片**，但在此示例中，同一个键**'照片'**用于发送两个文件。

```
curl --location --request PUT 'http://localhost:8080/employee' \
--header 'Content-Type: multipart/form-data' \
--form 'name=John' \
--form 'age=23' \
--form 'photo=@test1.png' \
--form 'photo=@test2.png'
```

**输出**

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

这次将保存三个文件，文件名如下。由于**FormFile**只访问给定键下的第一个文件，因此只有**formfile-test1.png**会通过**FormFile**方法保存。

```
formfile-test1.png
mapaccess-test2.png
mapaccess-test2.png
```

让我们看看另一个curl示例。

+   发送两个 **name** 键的值

```
curl --location --request PUT 'http://localhost:8080/employee' \
--header 'Content-Type: multipart/form-data' \
--form 'name=John' \
--form 'name=John2' \
--form 'age=23' \
--form 'photo=@test.png'
```

**输出**

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

rqeuest.**PostFormValue** 返回

```
John
```

而所有其他选项则返回两个值

```
[John John2]
```

# **HTTP 客户端示例**

以下是一个 HTTP 客户端示例代码，它在 HTTP 请求中将 **multipart/form-data** 请求体发送到上面示例中创建的服务器。

首先，我们必须创建一个多部分写入器 [https://golang.org/pkg/mime/multipart/#Writer](https://golang.org/pkg/mime/multipart/#Writer)

```
writer := multipart.NewWriter(body)
```

多部分写入器提供两种方法

+   **CreateFormField** - 用于创建要在多部分请求体中发送的文本字段。我们将使用此方法创建 **name** 和 **age** 字段。

+   **CreateFormFile** - 用于创建要在多部分请求体中发送的文件字段。我们将使用此方法创建 **photo** 字段。

现在让我们看看程序

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

运行这个程序，观察在服务器端的输出，因为上述客户端会调用你在第一个示例中创建的服务器。服务器程序将给出类似于 curl 调用的输出。

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

此外，服务器将保存两个文件，名称与 curl 调用类似

```
formfile-test.png
mapaccess-test.png
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
