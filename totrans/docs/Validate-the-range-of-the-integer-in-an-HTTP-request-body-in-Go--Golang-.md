<!--yml
category: 未分类
date: 2024-10-13 06:34:06
-->

# Validate the range of the integer in an HTTP request body in Go (Golang)

> 来源：[https://golangbyexample.com/validate-range-http-body-golang/](https://golangbyexample.com/validate-range-http-body-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

The below library can be used to validate the range of an integer in an incoming JSON HTTP request body

*   **gopkg.in/go-playground/validator.v9** – [https://pkg.go.dev/github.com/go-playground/validator](https://pkg.go.dev/github.com/go-playground/validator)

For this tutorial, we will try to parse the incoming JSON request body into the below employee struct

```
type employee struct {
    Age int `validate:"required,gte=10,lte=20"`
}
```

Notice here that we need to associate meta tags with fields of the struct to let the validator know that you want to validate this field. In the above example, we added the tag with the Age field. This tag is interpreted by the playground validate library. Notice we added three validations for the **Age** field

*   **required** – validates that the field is present

*   **gte** – validate that the field value is greater than equal to a particular value

*   **lte** – validate that the field value is less than equal to a particular value

# **Program**

```
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

Run the program.

```
go run main.go
```

It will start an HTTP server listening on port 8080\. Now make some curl API calls

*   Below curl call

```
curl  -X POST -H "content-type: application/json" http://localhost:8080/employee -d '{"Age": 5}'
```

gives below response as 5 is less than the minimum which is 10

```
{"message":"Bad Request Key: 'employee.Age' Error:Field validation for 'Age' failed on the 'gte' tag"}
```

*   Below curl call

```
curl  -X POST -H "content-type: application/json" http://localhost:8080/employee -d '{"Age": 10}'
```

gives below response as 10 is greater than the maximum which is 20

```
{"message":"Bad Request Key: 'employee.Age' Error:Field validation for 'Age' failed on the 'lte' tag"}
```

*   Below curl call

```
curl  -X POST -H "content-type: application/json" http://localhost:8080/employee -d '{"Age": 15}'
```

gives response as success as 15 is greater than 10 and less than 20.

```
{"message":"Success"}
```*