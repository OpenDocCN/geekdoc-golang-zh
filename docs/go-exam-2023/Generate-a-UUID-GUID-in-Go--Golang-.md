<!--yml

分类：未分类

日期：2024-10-13 06:09:07

-->

# 在 Go (Golang) 中生成一个 UUID/GUID

> 来源：[https://golangbyexample.com/generate-uuid-guid-golang/](https://golangbyexample.com/generate-uuid-guid-golang/)

UUID 也称为 GUID，是一个 16 字节或 128 位的数字。它旨在唯一标识某个事物。UUID 的一些属性包括

+   UUID 很可能与生成的其他 UUID 不同。实际上，如果你生成 10 万亿个 UUID，两个 UUID 相同的概率为 .00000006。

+   UUID 的生成不依赖于任何中央服务器。

+   UUID 表示为 32 个十六进制数字（基数 16），以 5 个部分用连字符分隔。格式为 8-4-4-4-12。因此总共有 36 个字符，其中包括 32 个十六进制数字和 4 个连字符。以下是一个示例

```
705e4dcb-3ecd-24f3-3a35-3e926e4bded5
```

有不同的库可用于生成 UUID。让我们看看两个可以用来生成 UUID 的库

[https://github.com/google/uuid](https://github.com/google/uuid)

**代码：**

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

**输出**：

```
cda6498a-235d-4f7e-ae19-661d41bc154c
cda6498a235d4f7eae19661d41bc154c
```

[https://github.com/pborman/uuid](https://github.com/pborman/uuid)

**代码**

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

**输出：**

```
cda6498a-235d-4f7e-ae19-661d41bc154c
cda6498a235d4f7eae19661d41bc154c
```
