<!--yml
category: 未分类
date: 2024-10-13 06:09:07
-->

# Generate a UUID/GUID in Go (Golang)

> 来源：[https://golangbyexample.com/generate-uuid-guid-golang/](https://golangbyexample.com/generate-uuid-guid-golang/)

UUID also known as GUID is a 16 byte or 128-bit number. It is meant to uniquely identify something. Some of the properties of UUID are

*   UUID is likely to be different from any other UUID generated. In fact, there will be .00000006 chance that two UUID will be the same in case you were to generate 10 trillion UUID

*   UUID doesn’t depend upon any central server for their generation

*   UUID is represented as 32 hexadecimal digits(Base 16) displayed in 5 parts separated by hyphens. The form is 8-4-4-4-12\. So there are a total of 36 characters which include 32 hexadecimal and 4 hyphens. Below is one sample

```
705e4dcb-3ecd-24f3-3a35-3e926e4bded5
```

There are different libraries available for generating the UUID. Let’s see two libraries which can be used to generate UUID

[https://github.com/google/uuid](https://github.com/google/uuid)

**Code:**

```
package main

import (
    "fmt"
    "strings"
    "github.com/google/uuid"
)

func main() {
    uuidWithHyphen := uuid.New()
    fmt.Println(uuidWithHyphen)
    uuid := strings.Replace(uuidWithHyphen.String(), "-", "", -1)
    fmt.Println(uuid)
}
```

**Output**:

```
cda6498a-235d-4f7e-ae19-661d41bc154c
cda6498a235d4f7eae19661d41bc154c
```

[https://github.com/pborman/uuid](https://github.com/pborman/uuid)

**Code**

```
package main

import (
	"fmt"
	"strings"

	"github.com/pborman/uuid"
)

func main() {
	uuidWithHyphen := uuid.NewRandom()
	fmt.Println(uuidWithHyphen)
	uuid := strings.Replace(uuidWithHyphen.String(), "-", "", -1)
	fmt.Println(uuid)
} 
```

**Output:**

```
cda6498a-235d-4f7e-ae19-661d41bc154c
cda6498a235d4f7eae19661d41bc154c
```