<!--yml

类别：未分类

日期：2024-10-13 06:32:23

-->

# Go（Golang）中的 Base64 编码/解码

> 来源：[https://golangbyexample.com/base64-golang/](https://golangbyexample.com/base64-golang/)

目录

**[概述](#Overview "Overview")**

+   [标准编码](#StdEncoding "StdEncoding")

+   [URL编码](#URLEncoding "URLEncoding")

+   [原始标准编码](#RawStdEncoding "RawStdEncoding")

+   [原始 URL 编码](#RawURLEncoding "RawURLEncoding")*  *# **概述**

Golang 提供了一个 **encoding/base64** 包，可用于将字符串编码为 base64 并将 base64 编码的字符串解码回原始字符串。[https://golang.org/pkg/encoding/base64/](https://golang.org/pkg/encoding/base64/)

Go 支持四种不同方式的 base64。

+   **标准编码**– 标准的 64 个字符，带有填充。它编码为以下字符

```go
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/
```

+   **URL编码** – 与标准编码相同，只是其中的 **‘+’** 和 **‘\’** 符号被替换为 **‘-‘** 和 **‘_’**。它们被替换以使其与文件名和 URL 兼容。以下是 base64 URL 编码的字符集

```go
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_
```

+   **原始标准编码** – 这与标准编码相同，只是省略了填充字符。因此，它是未填充的 base64 标准编码。

+   **原始 URL 编码** – 这也与 URL 编码相同，只是省略了填充字符。因此，它是未填充的 base64 URL 编码。

上述每个编码由 Encoding 结构表示

[https://golang.org/pkg/encoding/base64/#Encoding](https://golang.org/pkg/encoding/base64/#Encoding)

Encoding 结构进一步定义了两个编码和解码的方法

+   **编码为字符串** – 以下是该方法的签名。它将字节作为输入，并根据使用的四种编码之一返回 base64 编码的字符串。

```go
func (enc *Encoding) EncodeToString(src []byte) string
```

+   **解码字符串** – 以下是该方法的签名。它将编码字符串作为输入，并根据使用的四种编码之一返回原始字符串。

```go
func (enc *Encoding) DecodeString(s string) ([]byte, error)
```

让我们详细看看每一个例子

# **标准编码**

这是标准的 base64 编码，定义在 RFC 4648 中。它使用以下 64 个字符集以及填充字符 **‘=’**。

```go
Value Encoding  Value Encoding  Value Encoding  Value Encoding
         0 A            17 R            34 i            51 z
         1 B            18 S            35 j            52 0
         2 C            19 T            36 k            53 1
         3 D            20 U            37 l            54 2
         4 E            21 V            38 m            55 3
         5 F            22 W            39 n            56 4
         6 G            23 X            40 o            57 5
         7 H            24 Y            41 p            58 6
         8 I            25 Z            42 q            59 7
         9 J            26 a            43 r            60 8
        10 K            27 b            44 s            61 9
        11 L            28 c            45 t            62 +
        12 M            29 d            46 u            63 /
        13 N            30 e            47 v
        14 O            31 f            48 w         (pad) =
        15 P            32 g            49 x
        16 Q            33 h            50 y
```

让我们看一个这个的例子

**例子**

```go
package main

import (
	"encoding/base64"
	"fmt"
	"log"
)

func main() {
	sample := "a@"
	encodedString := base64.StdEncoding.EncodeToString([]byte(sample))
	fmt.Println(encodedString)

	originalStringBytes, err := base64.StdEncoding.DecodeString(encodedString)
	if err != nil {
		log.Fatalf("Some error occured during base64 decode. Error %s", err.Error())
	}
	fmt.Println(string(originalStringBytes))
}
```

**输出**

```go
YUA=
a@
```

请注意，在上面的例子中，我们使用 base64 标准编码对下面的字符串进行编码

```go
sample := "a@"
encodedString := base64.StdEncoding.EncodeToString([]byte(sample))
```

然后我们将 base64 编码的字符串解码回原始字符串

```go
originalStringBytes, err := base64.StdEncoding.DecodeString(encodedString)
if err != nil {
	log.Fatalf("Some error occured during base64 decode. Error %s", err.Error())
}
fmt.Println(string(originalStringBytes))
```

它在解码时正确输出原始字符串

# **URL编码**

在这个例子中，**‘+’** 和 **‘\’** 符号被替换为 **‘-‘** 和 **‘_’**。它们被替换以使其与文件名和 URL 兼容。

**URL**：在 URL 中，**‘+’** 和 **‘\’** 由于 URL 编码而进一步编码为十六进制序列，从而进一步增加了 URL 的长度。例如，**‘+’** 将被转换为 ‘%2B’，**‘\’** 将在 URL 编码中编码为 ‘%2F’。

**文件名**：在 Unix 和 Windows 中，文件路径使用 **‘\’**。因此，它将 **‘\’** 替换为 **‘_’**。

让我们看一个例子

```go
package main

import (
	"encoding/base64"
	"fmt"
	"log"
)

func main() {
	sample := "�"

	encodedStringURL := base64.URLEncoding.EncodeToString([]byte(sample))
	fmt.Printf("URL Encoding: %s\n", encodedStringURL)
	encodedStringSTD := base64.StdEncoding.EncodeToString([]byte(sample))
	fmt.Printf("STD Encoding: %s\n", encodedStringSTD)

	originalStringBytes, err := base64.URLEncoding.DecodeString(encodedStringURL)
	if err != nil {
		log.Fatalf("Some error occured during base64 decode. Error %s", err.Error())
	}
	fmt.Println(string(originalStringBytes))
}
```

**输出**

```go
URL Encoding: 77-9
STD Encoding: 77+9
�
```

注意在上述示例中，我们取了下面字符串的示例

```go
sample := "�"
```

然后我们打印上述字符串的标准编码和 URL 编码

```go
encodedStringURL := base64.URLEncoding.EncodeToString([]byte(sample))
fmt.Printf("URL Encoding: %s\n", encodedStringURL)
encodedStringSTD := base64.StdEncoding.EncodeToString([]byte(sample))
fmt.Printf("STD Encoding: %s\n", encodedStringSTD)
```

注意输出中的区别

对于 URL 编码，它输出

```go
77-9
```

而对于标准编码，它输出

```go
77+9
```

这是因为在标准编码中存在 **‘+’**，而在 URL 编码中 **‘+’** 被替换为 **‘-‘**

# **原始标准编码**

这与标准编码相同，只是省略了填充字符。因此这是未填充的标准 base64 编码。让我们看一个例子。在我们看到原始标准编码的例子之前，我们想解释为什么存在省略填充字符的标准编码的原始版本。这涉及到一个非常重要的主题，即 base64 编码中的 **‘填充是否必要’**。答案是这取决于情况。

+   当你发送单个字符串时，填充不是必需的。

+   当你串联多个字符串的 base64 编码时，填充是重要的。如果未填充的字符串被串联，那么将不可能得到原始字符串，因为关于添加的字节的信息将会丢失。作为说明，请考虑以下内容

| **实际字符串** | **带填充的 Base64 编码** | **不带填充的 Base64 编码** |
| --- | --- | --- |
| a | YQ== | YQ |
| bc | YmM= | YmM |
| def | ZGVm | ZGVm |

现在让我们考虑这两种情况。

**当串联发送时不需要填充**

在这种情况下，串联的 Base64 字符串将是

```go
YQYmMZGVm
```

尝试解码它，你将得到如下的最终字符串，这是不正确的

```go
a&1
```

**当串联发送时需要填充**

在这种情况下，串联的 Base64 字符串将是

```go
YQ==YmM=ZGVm
```

尝试按 4 个字符分组解码，你将得到如下的最终字符串，这是正确的

```go
abcdef
```

现在再次浮现的问题是，为什么需要串联多个 base64 编码的字符串。答案是，在有流数据的情况下，想要随着数据的到来发送 base64 编码的数据总是好的。例如，视频的缓冲。

所以这就是为什么填充是被鼓励的，尽管在所有情况下并非绝对必要。现在让我们看一个原始标准编码的例子。

```go
package main

import (
	"encoding/base64"
	"fmt"
	"log"
)

func main() {
	sample := "a@"
	encodedStringStdEncoding := base64.StdEncoding.EncodeToString([]byte(sample))
	fmt.Printf("STD Encoding: %s\n", encodedStringStdEncoding)

	encodedStringRawStdEncoding := base64.RawStdEncoding.EncodeToString([]byte(sample))
	fmt.Printf("Raw STD Encoding: %s\n", encodedStringRawStdEncoding)

	originalStringBytes, err := base64.RawStdEncoding.DecodeString(encodedStringRawStdEncoding)
	if err != nil {
		log.Fatalf("Some error occured during base64 decode. Error %s", err.Error())
	}
	fmt.Println(string(originalStringBytes))
}
```

**输出**

```go
STD Encoding: YUA=
Raw STD Encoding: YUA
a@
```

在上面的示例中，我们打印了 **标准编码** 和 **原始标准编码** 的结果。

```go
encodedStringStdEncoding := base64.StdEncoding.EncodeToString([]byte(sample))
fmt.Printf("STD Encoding: %s\n", encodedStringStdEncoding)

encodedStringRawStdEncoding := base64.RawStdEncoding.EncodeToString([]byte(sample))
fmt.Printf("Raw STD Encoding: %s\n", encodedStringRawStdEncoding)
```

注意输出中的区别

对于标准编码，它输出

```go
YUA=
```

而对于原始标准编码，它输出

```go
YUA
```

正如你所注意到的，原始标准编码省略了填充字符。

# **原始 URL 编码**

这与 URL 编码相同，只是它省略了填充字符。所以这是未填充的标准 base64 编码。

```go
package main
import (
    "encoding/base64"
    "fmt"
    "log"
)
func main() {
    sample := "a@"
    encodedStringURLEncoding := base64.URLEncoding.EncodeToString([]byte(sample))
    fmt.Printf("URL Encoding: %s\n", encodedStringURLEncoding)

    encodedStringRawURLEncoding := base64.RawURLEncoding.EncodeToString([]byte(sample))
    fmt.Printf("Raw URL Encoding: %s\n", encodedStringRawURLEncoding)

    originalStringBytes, err := base64.RawStdEncoding.DecodeString(encodedStringRawURLEncoding)
    if err != nil {
        log.Fatalf("Some error occured during base64 decode. Error %s", err.Error())
    }
    fmt.Println(string(originalStringBytes))
}
```

**输出**

```go
URL Encoding: YUA=
Raw URL Encoding: YUA
a@
```

在上述示例中，我们再次打印了 URL 编码和原始 URL 编码的结果，你可以注意到填充字符 **‘=’** 在原始 URL 编码中被省略了。
