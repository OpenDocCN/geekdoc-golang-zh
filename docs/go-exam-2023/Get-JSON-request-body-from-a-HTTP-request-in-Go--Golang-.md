<!--yml

category: 未分类

date: 2024-10-13 06:31:52

-->

# 从HTTP请求中获取JSON请求体（Golang）

> 来源：[https://golangbyexample.com/json-request-body-golang-http/](https://golangbyexample.com/json-request-body-golang-http/)

目录

**   [概述](#Overview "Overview")

+   [示例](#Example "Example")*  *# **概述**

**json/encoding**包包含可以用于将传入HTTP请求的请求体转换为Go语言结构体的方法。在开始之前，先说一句关于请求体的话。HTTP请求的请求体是一系列字节。请求体的内容类型表示这些字节的表示格式，并且意味着要被读取。对于JSON请求体，内容类型是

```
application/json
```

另外还有两件关于Go语言结构体你需要知道的事。

+   仅导出的结构体字段对外部库可见。因此，只有导出的结构体字段才能从传入的HTTP请求中解析。需要注意的是，Go语言中大写的结构体字段是导出的。

+   结构体字段有一个元信息部分，包含关于该字段的附加信息。这些元字段在将传入的JSON请求体解析为结构体时使用。例如，假设我们有以下结构体：

```
type employee struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}
```

注意与每个标记为‘json’的字段相关联的元标签。这些元字段用于将JSON中的键映射到结构体的字段。例如，如果我们有以下JSON：

```
{
  "name" : "John",
  "age"  : 21
}
```

然后上述JSON的**name**键将映射到**employee**结构体的**Name**字段，JSON中的**age**键将映射到结构体的**Age**字段。假设我们有以下结构体和JSON：

```
type employee struct {
	Name string `json:"n"`
	Age  int    `json:"ag"`
}

{
  "n" : "John",
  "age"  : 21
}
```

然后JSON中的**‘n’**键将映射到结构体的**Name**字段，JSON中的**‘ag’**键将映射到结构体的**Age**字段。

可以使用**json/encoding**包的以下两个方法获取传入请求的JSON请求体。

+   json.Unmarshal([]byte)

+   json.NewDecoder(io.Reader).Decode(interface{})

第二种方法是获取JSON请求体的优选方式，原因有两个。

+   传入HTTP请求的请求体是一个io流。**json.Unmarshal**将首先读取请求体的全部内容，然后进行解组。它不会对请求体进行验证，而是直接处理。相比之下，Decode方法如果JSON无效会立即给出解析错误。这对于客户端发送的大型无效请求体非常有用。Unmarshal将在整个大请求体读取完毕后发现问题，而Decode在一开始就会抛出错误。

+   **json.Decode**包含**DisallowUnknownFields**方法，如果传入的JSON包含与任何字段不匹配的键，则会引发错误。

1.  导出和

1.  未被忽略的结构体字段。

在解析传入的JSON请求体时，可能需要处理几个问题。

+   请求体可能不是有效的JSON。

+   这是一个有效的JSON，但包含额外的字段或没有任何结构体中预期的字段。

+   JSON 请求体太大。

+   该字段是预期的，但它包含不同的类型。例如，对于上面的名称字段，传入的 HTTP 请求 JSON 主体有一个数字，而预期是一个字符串。

+   请求体包含额外字段。

**Decode** 方法可以捕获所有这些问题，并能够返回适当的错误消息，除了在出现解组错误的情况下。让我们看看在所有情况下解码方法返回的错误。

想象一下，传入的 JSON 需要转换成我们上面提到的**员工**结构。

问题

+   传入的 JSON 中有一个额外的字段。例如，假设我们有一个额外的**性别**字段。

```
'{"Name":"John", "Age": 21, "Gender": "M"}'
```

解码函数返回的错误将是

```
unknown field "Gender"
```

+   请求体不是有效的 JSON

```
'{"Name": "John", "Age":}'
```

解码函数返回的错误将是

```
invalid character '}' looking for beginning of value
```

+   请求体为空

```
''
```

解码函数返回的错误将是

```
EOF
```

+   某个字段的类型与预期不同。例如，发送一个字符串值的年龄，而预期是 int。

```
'{"Name":"John", "Age": "21"}'
```

解码函数返回的错误将是

```
json: cannot unmarshal string into Go struct field employee.age of type int
```

在这种情况下，解码函数返回的错误包含内部信息，这不是应返回给客户端的适当错误。也可以捕获这种类型的错误并返回适当的错误给客户端。这正是我们在下面的程序中所做的。只需检查是否是解组错误，然后返回自定义错误消息。

在下面的代码中，我们还设置了**解码器**的禁止未知字段选项，因此传入 JSON 主体中的任何额外字段都会导致错误。

```
decore.DisallowUnknownFields()
```

# **示例**

下面是完整的程序示例。

```
package main

import (
	"encoding/json"
	"errors"
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
	if headerContentTtype != "application/json" {
		errorResponse(w, "Content Type is not application/json", http.StatusUnsupportedMediaType)
		return
	}
	var e employee
	var unmarshalErr *json.UnmarshalTypeError

	decoder := json.NewDecoder(r.Body)
	decoder.DisallowUnknownFields()
	err := decoder.Decode(&e)
	if err != nil {
		if errors.As(err, &unmarshalErr) {
			errorResponse(w, "Bad Request. Wrong Type provided for field "+unmarshalErr.Field, http.StatusBadRequest)
		} else {
			errorResponse(w, "Bad Request "+err.Error(), http.StatusBadRequest)
		}
		return
	}
	errorResponse(w, "Success", http.StatusOK)
	return
}

func errorResponse(w http.ResponseWriter, message string, httpStatusCode int) {
	w.Header().Set("Content-Type", "application/json")
	w.WriteHeader(httpStatusCode)
	resp := make(map[string]string)
	resp["message"] = message
	jsonResp, _ := json.Marshal(resp)
	w.Write(jsonResp)
}
```

运行上述文件。这将触发一个服务器，监听端口 8080。在服务器运行后，让我们对我们讨论过的一些场景进行 API 调用。

+   **正确的请求**

```
curl -X POST -H "content-type: application/json" http://localhost:8080/employee -d '{"Name":"John", "Age": 21}'
```

**输出**

```
Response Code: 200
Response Body: {"message":"Success"}
```

+   **传入的 JSON 中有一个额外的字段**

```
curl -v -X POST -H "content-type: application/json" http://localhost:8080/employee -d '{"Name":"John", "Age": 21, "Gender": "M"}'
```

**输出**

```
Response Code: 400
Response Body: {"message":"Bad Request json: unknown field \"Gender\""}
```

+   **请求体不是有效的 JSON**

```
curl -v -X POST -H "content-type: application/json" http://localhost:8080/employee -d '{"Name": "John", "Age":}'
```

**输出**

```
Response Code: 400
Response Body: {"message":"Bad Request invalid character '}' looking for beginning of value"}
```

+   **请求体为空**

```
curl -v -X POST -H "content-type: application/json" http://localhost:8080/employee
```

**输出**

```
Response Code: 400
Response Body: {"message":"Bad Request EOF"}
```

+   **某个字段的类型与预期不同。例如，发送一个字符串值的年龄，而预期是 int**

```
curl -v -X POST -H "content-type: application/json" http://localhost:8080/employee -d '{"Name":"John", "Age": "21"}'
```

**输出**

```
Response Code: 400
Response Body: {"message":"Bad Request. Wrong Type provided for field age"}
```

+   **未提供内容类型或内容类型不是 application/json**

```
curl -X POST -H "content-type: application/json" http://localhost:8080/employee -d '{"Name":"John", "Age": 21}'
```

**输出**

```
Response Code: 415 Unsupported Media Type
Response Body: {"message":"Content Type is not application/json"}
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [http](https://golangbyexample.com/tag/http/)*   [json](https://golangbyexample.com/tag/json/)*
