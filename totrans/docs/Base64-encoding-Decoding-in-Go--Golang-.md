<!--yml
category: 未分类
date: 2024-10-13 06:32:23
-->

# Base64 encoding/Decoding in Go (Golang)

> 来源：[https://golangbyexample.com/base64-golang/](https://golangbyexample.com/base64-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [StdEncoding](#StdEncoding "StdEncoding")
*   [URLEncoding](#URLEncoding "URLEncoding")
*   [RawStdEncoding](#RawStdEncoding "RawStdEncoding")
*   [RawURLEncoding](#RawURLEncoding "RawURLEncoding")*  *# **Overview**

Golang provides an **encoding/base64** package that can be used to encode strings to base64 and decode the base64 encoded string back to the original string. [https://golang.org/pkg/encoding/base64/](https://golang.org/pkg/encoding/base64/)

Go supports base64 in 4 different ways.

*   **StdEncoding**– Standard 64 characters with padding. It encodes into below characters

```
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/
```

*   **URLEncoding** –  Same as StdEncoding just that in this, the **‘+’** and **‘\’** sign is replaced with **‘-‘** and **‘_’**. They are replaced to make them compatible with filenames and URLs. Below is the character set for base64 URL encoding

```
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_
```

*   **RawStdEncoding** – This is the same as StdEncoding just that it omits padding characters.  So it is unpadded base64  STD encoding.

*   **RawURLEncoding** – This is also the same as URL encoding just that it omits padding characters. So it is unpadded base64 URL encoding.

Each of the above encodings is represented by the Encoding struct

[https://golang.org/pkg/encoding/base64/#Encoding](https://golang.org/pkg/encoding/base64/#Encoding)

Encoding struct further defines two methods for encoding and decoding

*   **EncodeToString** – Below is the signature of the method. It takes bytes as an input and returns the base64 encoded string depending upon which one of the above 4 encodings is used.

```
func (enc *Encoding) EncodeToString(src []byte) string
```

*   **DecodeString** – Below is the signature of the method. It takes the encoded string as an input and returns the original string depending upon which one of the above 4 encodings is used.

```
func (enc *Encoding) DecodeString(s string) ([]byte, error)
```

Let’s see a detailed example of each

# **StdEncoding**

This is the standard base64 encoding which is defined in RFC 4648\. It uses the below 64 characters set along with padding character **‘=’.**

```
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

Let’s see an example of this

**Example**

```
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

**Output**

```
YUA=
a@
```

Notice that in the above example we are encoding the below string using base64 Std encoding

```
sample := "a@"
encodedString := base64.StdEncoding.EncodeToString([]byte(sample))
```

Then we are decoding the base64 encoded string back to the original string

```
originalStringBytes, err := base64.StdEncoding.DecodeString(encodedString)
if err != nil {
	log.Fatalf("Some error occured during base64 decode. Error %s", err.Error())
}
fmt.Println(string(originalStringBytes))
```

It correctly outputs the original string back on decoding

# **URLEncoding**

In this, the **‘+’** and **‘\’** sign is replaced with **‘-‘** and **‘_’**. They are replaced to make them compatible with filenames and URLs. 

**URL**: In URLs **‘+’** and **‘\’**  are further encoded into hexadecimal sequences due to URL encoding thereby further increasing the length of the URL. For eg **‘+’** will be converted to ‘%2B’ and **‘\’** will be encoded to ‘%2F’ in the URL encoding.

**Filenames**:  **‘\’** is used in file paths in both Unix and Windows. Hence it ‘\’ is replaced with **‘_’**

Let’s see an example

```
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

**Output**

```
URL Encoding: 77-9
STD Encoding: 77+9
�
```

Notice that in the above example we have taken the example of the below string

```
sample := "�"
```

Then we are printing both the StdEncoding as well as URL Encoding of the above string

```
encodedStringURL := base64.URLEncoding.EncodeToString([]byte(sample))
fmt.Printf("URL Encoding: %s\n", encodedStringURL)
encodedStringSTD := base64.StdEncoding.EncodeToString([]byte(sample))
fmt.Printf("STD Encoding: %s\n", encodedStringSTD)
```

Notice the difference in output

For URL encoding it prints

```
77-9
```

while for STD encoding, it prints

```
77+9
```

This is because **‘+’** is present in STD encoding but in URL encoding **‘+’** is replaced with **‘-‘**

# **RawStdEncoding**

This is the same as StdEncoding just that it omits padding characters.  So it is unpadded STD base64 encoding. Let’s see an example. Before we see an example of Raw STD encoding we want to explain why the raw version of std encoding exists which omits padding characters.  That brings a very important topic related to base64 encoding which is **‘Is padding necessary’.** The answer is that it depends

*   Padding is not necessary when you are sending a single string.

*   Padding is important when you are concatenating base64 encodings of multiple strings. If unpadded strings are concatenated then it will be impossible to get the original string as information about the bytes added will be lost. As an illustration, consider below

| **Actual String** | **Base64 Encoded with Padding** | **Base64 Encoded without Padding** |
| a | YQ== | YQ |
| bc | YmM= | YmM |
| def | ZGVm | ZGVm |

Now let’s consider both cases.

**When the concatenation is sent without padding**

In this case, the concatenated Base64 string will be

```
YQYmMZGVm
```

Try decoding it and you will get the final string as below which is incorrect

```
a&1
```

**When the concatenation is sent with padding**

In this case, the concatenated Base64 string will be

```
YQ==YmM=ZGVm
```

Try decoding it in groups of 4 characters and you will get the final string as below which is correct

```
abcdef
```

Now again the question that comes to mind is why do you need to concatenate multiple base64 encoded strings. The answer is that it is always good in cases where there is streaming data and you want to send the base64 encoded data as it comes. For example, buffering of video.

So that is why padding is encouraged though not absolutely not necessary in all cases. Now let’s see an example of Raw Std Encoding

```
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

**Output**

```
STD Encoding: YUA=
Raw STD Encoding: YUA
a@
```

In the above example we are printing both the result of both **StdEncoding** as well as **RawStdEncoding**

```
encodedStringStdEncoding := base64.StdEncoding.EncodeToString([]byte(sample))
fmt.Printf("STD Encoding: %s\n", encodedStringStdEncoding)

encodedStringRawStdEncoding := base64.RawStdEncoding.EncodeToString([]byte(sample))
fmt.Printf("Raw STD Encoding: %s\n", encodedStringRawStdEncoding)
```

Notice the difference in output

For STD encoding it prints

```
YUA=
```

while for Raw STD encoding, it prints

```
YUA
```

As you can notice that RAW STD Encoding omits padding characters.

# **RawURLEncoding**

This is also the same as URL encoding just that it omits padding characters. So it is unpadded STD base64 encoding.

```
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

 **Output**

```
URL Encoding: YUA=
Raw URL Encoding: YUA
a@
```

In the above example, we are again printing the result of both URL encoding and as well as RAW URL encoding and you can notice that padding character **‘=’** is omitted in the Raw URL encoding.*