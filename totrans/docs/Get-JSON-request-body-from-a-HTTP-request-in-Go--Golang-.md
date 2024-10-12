<!--yml
category: 未分类
date: 2024-10-13 06:31:52
-->

# Get JSON request body from a HTTP request in Go (Golang)

> 来源：[https://golangbyexample.com/json-request-body-golang-http/](https://golangbyexample.com/json-request-body-golang-http/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

**json/encoding** package contains methods that can be used to convert a  request body of an incoming HTTP request into a golang struct. Before we start just a word about the request body. The request body of an HTTP request is a sequence of bytes. It is the content-type of the HTTP request which denotes the format in which those bytes are represented and meant to be read back.  For a JSON request body, the content-type is

```
application/json
```

Also two things about golang struct that you need to know

*   Only exported fields of the struct are visible to the external library. Hence only exported fields of the struct can be parsed into from an incoming HTTP request. Also to note that capitalized fields of a struct are exported in go.

*   Struct fields have a meta section that contains additional information about that field. These meta fields are used while parsing an incoming JSON request body into a struct. For eg let’s say we have the below struct

```
type employee struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}
```

Notice meta tags associated with each of the fields annotated with name as ‘json’. These meta fields are used to map keys in the JSON to the fields of the struct. For eg if we have JSON as

```
{
  "name" : "John",
  "age"  : 21
}
```

Then **name** key of the above JSON will map to the **Name** field of the **employee** struct and the **age** key in JSON will map to the **Age** field of the struct. Let’s say we have below struct and JSON

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

Then **‘n’** key of the JSON will map to the **Name** field of the struct and **‘ag’** key of the JSON will map to the **Age** field of the struct.

Below two methods of the **json/encoding** package can be used to get the JSON request body of an incoming request.

*   json.Unmarshal([]bye)

*   json.NewDecoder(io.Reader).Decode(interface{})

The second method is a preferred way to get the json request body for two reasons.

*   The request body of an incoming HTTP request is an io stream. **json.Unmarshal** will read the entire contents of the body first and then do an Unmarshal. It does no validation on the request body as it comes along. As compared Decode method will give an early parse error if the JSON is not valid. This comes in handy for a large invalid body being sent by the client. Unmarshal will detect that when the entire large body has been read while Decode will raise at the start only.

*   **json.Decode** contains the **DisallowUnknownFields** method which raises an error if the incoming JSON contains keys which do not match any

1.  Exported and
2.  Non-ignored fields of the struct.

There might be several issues that need to be taken care of when parsing the incoming JSON request body.

*   The body might not be a valid JSON

*   It is a valid JSON but contains extra fields or none of the fields expected in the struct.

*   The JSON request body is too large.

*   The field is expected but it contains a different type. For example for the Name field above the incoming HTTP request JSON body has a number while the expectation is a string.

*   The req body contains extra fields.

The **Decode** method can capture all of those issues and can return appropriate error messages as well except in one case when there is an unmarshaling error. Let’s see the error returned by the decode method in all cases

Imaging the incoming JSON need to converted into the **employee** struct we mentioned above

Issues

*   There is an extra field in the incoming JSON. For example, let’s say we have an additional **Gender** field present.

```
'{"Name":"John", "Age": 21, "Gender": "M"}'
```

The error returned by decode function will be

```
unknown field "Gender"
```

*   The body is not a valid JSON

```
'{"Name": "John", "Age":}'
```

The error returned by the decode function will be

```
invalid character '}' looking for beginning of value
```

*   The body is empty

```
''
```

The error returned by decode function will be

```
EOF
```

*   The type of one of the field is different than expected. For example, sending a string value of age where int is expected

```
'{"Name":"John", "Age": "21"}'
```

The error returned by decode function will be

```
json: cannot unmarshal string into Go struct field employee.age of type int
```

In this case, the error returned by the decode function is returning internal information which is not an appropriate error to be returned back to the client. It is also possible to catch such type of error and return an appropriate error to the client. And that is what we are doing in the below program as well. Just  checking if it is an unmarshaling and returning custom error message after that

Also in the code below we are setting the disallow unknown fields option on the **decoder** so that any extra fields in the incoming JSON body results in an error.

```
decore.DisallowUnknownFields()
```

# **Example**

Below is full program for the same.

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

Run the above file. It will trigger a server that will listen to port 8080\. After the server is running, let’s make the API calls for some of the scenarios we have discussed above.

*   **Correct Request**

```
curl -X POST -H "content-type: application/json" http://localhost:8080/employee -d '{"Name":"John", "Age": 21}'
```

**Output**

```
Response Code: 200
Response Body: {"message":"Success"}
```

*   **There is an extra field in the incoming JSON**

```
curl -v -X POST -H "content-type: application/json" http://localhost:8080/employee -d '{"Name":"John", "Age": 21, "Gender": "M"}'
```

**Output**

```
Response Code: 400
Response Body: {"message":"Bad Request json: unknown field \"Gender\""}
```

*   **The body is not a valid JSON**

```
curl -v -X POST -H "content-type: application/json" http://localhost:8080/employee -d '{"Name": "John", "Age":}'
```

**Output**

```
Response Code: 400
Response Body: {"message":"Bad Request invalid character '}' looking for beginning of value"}
```

*   **The body is empty**

```
curl -v -X POST -H "content-type: application/json" http://localhost:8080/employee
```

**Output**

```
Response Code: 400
Response Body: {"message":"Bad Request EOF"}
```

*   **The type of one of the field is different than expected. For example, sending a string value of age where int is expected**

```
curl -v -X POST -H "content-type: application/json" http://localhost:8080/employee -d '{"Name":"John", "Age": "21"}'
```

**Output**

```
Response Code: 400
Response Body: {"message":"Bad Request. Wrong Type provided for field age"}
```

*   **Content-type is not provided or not application/json**

```
curl -X POST -H "content-type: application/json" http://localhost:8080/employee -d '{"Name":"John", "Age": 21}'
```

**Output**

```
Response Code: 415 Unsupported Media Type
Response Body: {"message":"Content Type is not application/json"}
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [http](https://golangbyexample.com/tag/http/)*   [json](https://golangbyexample.com/tag/json/)*