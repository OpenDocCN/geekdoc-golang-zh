<!--yml

分类：未分类

日期：2024-10-13 06:34:06

-->

# 在 Go (Golang) 中验证 HTTP 请求体中整数的范围。

> 来源：[`golangbyexample.com/validate-range-http-body-golang/`](https://golangbyexample.com/validate-range-http-body-golang/)

目录

+   概述

+   程序

# **概述**

以下库可用于验证传入 JSON HTTP 请求体中整数的范围。

+   **gopkg.in/go-playground/validator.v9** – [`pkg.go.dev/github.com/go-playground/validator`](https://pkg.go.dev/github.com/go-playground/validator)

在本教程中，我们将尝试将传入的 JSON 请求体解析为以下员工结构。

```go
type employee struct {
    Age int `validate:"required,gte=10,lte=20"`
}
```

请注意，我们需要将元标签与结构的字段关联，以便让验证器知道您希望验证此字段。在上述示例中，我们为年龄字段添加了标签。这个标签由 playground validate 库解释。请注意，我们为 **年龄** 字段添加了三个验证。

+   **必需** – 验证字段是否存在。

+   **gte** – 验证字段值是否大于等于特定值。

+   **lte** – 验证字段值是否小于等于特定值。

# **程序**

```go
package main
import (
    "encoding/json"
    "net/http"
    "github.com/go-playground/validator"
)
var validate *validator.Validate
type employee struct {
    Age int `validate:"required,gte=10,lte=20"`
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
    decoder := json.NewDecoder(r.Body)
    err := decoder.Decode(&e)
    if err != nil {
        errorResponse(w, "Bad Request "+err.Error(), http.StatusBadRequest)
    }
    err = validateStruct(e)
    if err != nil {
        errorResponse(w, "Bad Request "+err.Error(), http.StatusBadRequest)
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
func validateStruct(e employee) error {
    validate = validator.New()
    err := validate.Struct(e)
    if err != nil {
        return err
    }
    return nil
}
```

运行程序。

```go
go run main.go
```

它将启动一个监听在 8080 端口的 HTTP 服务器。现在进行一些 curl API 调用。

+   以下 curl 调用

```go
curl  -X POST -H "content-type: application/json" http://localhost:8080/employee -d '{"Age": 5}'
```

返回以下响应，因为 5 小于最小值 10。

```go
{"message":"Bad Request Key: 'employee.Age' Error:Field validation for 'Age' failed on the 'gte' tag"}
```

+   以下 curl 调用

```go
curl  -X POST -H "content-type: application/json" http://localhost:8080/employee -d '{"Age": 10}'
```

返回以下响应，因为 10 大于最大值 20。

```go
{"message":"Bad Request Key: 'employee.Age' Error:Field validation for 'Age' failed on the 'lte' tag"}
```

+   以下 curl 调用

```go
curl  -X POST -H "content-type: application/json" http://localhost:8080/employee -d '{"Age": 15}'
```

返回成功，因为 15 大于 10 且小于 20。

```go
{"message":"Success"}
```*
