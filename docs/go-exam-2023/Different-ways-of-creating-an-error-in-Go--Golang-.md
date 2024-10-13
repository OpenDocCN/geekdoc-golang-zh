<!--yml

类别：未分类

日期：2024-10-13 06:02:42

-->

# Go (Golang) 中创建错误的不同方式

> 来源：[https://golangbyexample.com/different-ways-of-creating-an-error-in-go-golang/](https://golangbyexample.com/different-ways-of-creating-an-error-in-go-golang/)

在学习 Go 中创建错误的不同方式之前，首先了解错误。错误是一个具有以下签名的接口类型。

```go
type error interface {  
    Error() string
}
```

根据 **interface** 的定义，任何实现 **Error()** 方法的类型都成为 **error** 类型。

让我们看看创建 **error** 的不同方法

# 1\. **使用 errors.New(“some_error_message”)**

```go
package main

import (
    "errors"
    "fmt"
)

func main() {
    sampleErr := errors.New("error occured")
    fmt.Println(sampleErr)
}
```

**输出：**

```go
error occured
```

# 2\. **使用 fmt.Errorf(“error is %s”, “some_error_message”)。这种方式可以创建带格式的错误信息**

```go
package main

import (
    "fmt"
)

func main() {
    sampleErr := fmt.Errorf("Err is: %s", "database connection issue")
    fmt.Println(sampleErr)
}
```

**输出：**

```go
Err is: database connection issue
```

# 3\. **创建自定义错误**

下面的示例说明了自定义错误的使用。在下面的示例中

+   **inputError** 是一个结构体，具有 **Error()** 方法，因此它属于 **error** 类型。

+   你还可以通过扩展其字段或添加新方法来向自定义错误添加额外信息。**inputError** 有一个名为 **missingFields** 的附加字段和一个 **getMissingFields** 函数。

+   我们可以使用类型断言将 **error** 转换为 **inputError**

**示例：**

```go
package main

import "fmt"

type inputError struct {
    message      string
    missingField string
}

func (i *inputError) Error() string {
    return i.message
}

func (i *inputError) getMissingField() string {
    return i.missingField
}

func main() {
    err := validate("", "")
    if err != nil {
        if err, ok := err.(*inputError); ok {
            fmt.Println(err)
            fmt.Printf("Missing Field is %s\n", err.getMissingField())
        }
    }
}

func validate(name, gender string) error {
    if name == "" {
        return &inputError{message: "Name is mandatory", missingField: "name"}
    }
    if gender == "" {
        return &inputError{message: "Gender is mandatory", missingField: "gender"}
    }
    return nil
}
```

**输出：**

```go
Name is mandatory
Missing Field is name
```
