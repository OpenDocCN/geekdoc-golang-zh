<!--yml

分类：未分类

日期：2024-10-13 06:33:36

-->

# 关于 Base64 编码/解码的所有内容 – 完整指南

> 来源：[`golangbyexample.com/base64-complete-guide/`](https://golangbyexample.com/base64-complete-guide/)

目录

+   概述

+   为什么需要 Base64 编码

+   Base64 编码/解码如何工作

+   Base64 解码

+   填充是否必要

+   大小

+   Base64 替代方案

+   Base64 其他实现

+   Base64 应用

+   结论

# **概述**

Base64 也被称为 Base64 内容传输编码。Base64 是将二进制数据编码为 ASCII 文本。但它只使用 64 个字符和一个填充字符，这些字符在大多数字符集中都存在。因此，它是一种仅使用可打印字符表示二进制数据的方法。这些可打印字符为

+   小写字母 **a-z**

+   大写字母 **A-Z**

+   数字 **0-9**

+   字符 **+** 和 **/**

+   **=** 被用作填充字符

Base64 编码用于在不正确处理二进制数据的介质上传输数据。因此，对数据进行 Base64 编码是为了确保数据在通过该媒体时保持完整而不被修改。Base64 编码的样子如下。

```go
OWRjNGJjMDY5MzVmNGViMGExZTdkMzNjOGMwZTI3ZWI==
```

# **为什么需要 Base64 编码**

让我们谈谈历史，以了解为什么需要 base64。最初，当邮件系统启动时，仅传输文本。后来，电子邮件也开始支持附件，包括视频和音频。视频和音频是二进制数据，当通过互联网传输二进制数据时，有很大可能性会损坏。问题是为什么。一些媒体（如电子邮件）仅支持文本数据。它们仅用于流式文本，因此这些协议可能将二进制数据解释为控制字符，或者二进制数据中的某些特殊字符可能会被不同地解释。二进制数据的问题在于，二进制数据中的某些字符在某些媒体中可能具有特殊含义。换句话说，这些媒体不是 8 位干净，仅用于处理文本数据。

因此，base64 是为不支持二进制数据的媒体设计的，仅处理文本字符。保证数据不会损坏。它主要用于仅支持 ASCII 数据的遗留系统。

但为什么是 64 个字符而不是更多。这 64 个字符在大多数字符集中都有，因此可以合理地相信数据在另一端不会损坏。

注意，这是一种编码，而不是加密。编码和加密之间有什么区别？

**编码**

编码意味着将数据转换为另一种格式，以便可以被只理解转换格式的系统正确使用。编码只是一种公开可用的算法。没有涉及秘密密钥，并且是可逆的。因此，任何知道算法的人都可以简单地逆转并获取原始数据。现在问题是，为什么需要它。一些传输媒介只理解文本数据或一定数量的字符。您的二进制数据无法通过这样的媒介传输，因为存在数据损坏的可能性。

编码并不提供任何安全性，而只是为了在理解不同格式的不同媒体之间的兼容性。

**加密**

加密是为了保密。它始终由一个密钥保护，旨在由知道密钥的任何人进行解密。因此，在加密的情况下，数据只能由拥有密钥的人逆转。

例如，在网络上发送密码。在这种情况下，我们对密码进行加密，以便任何无意中的人无法读取它。例如 HTTPS。

因此，编码本质上是为了兼容性，而不是为了加密。

# **Base64 编码/解码的工作原理**

Base64 编码在[`tools.ietf.org/html/rfc4648`](https://tools.ietf.org/html/rfc4648)中有描述。

Base64 编码将把每 3 个字节的数据转换为 4 个编码字符。它将从左到右开始扫描，然后选择前 3 个字节的数据，表示 3 个字符。这 3 个字节将是 24 位。现在，它将把这 24 位分成四个部分，每部分 6 位。然后每个 6 位组将在下面的表中进行索引，以获取映射字符。

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

让我们用一个例子来理解。假设我们有以下字符串

```go
ab@
```

上述字符串的位表示为

```go
 a          b        @
01100001  01100010  01000000
```

这 24 位将被分组为 4 组，每组 6 位。

```go
011000 010110 001001 000000
```

上述位的数值表示为

```go
 24    22     9      0
011000 010110 001001 000000
```

使用上述数字在 Base64 表中进行索引。下面是映射。

| 24 | Y |
| --- | --- |
| 22 | W |
| 9 | J |
| 0 | A |

因此，Base64 编码的字符串将是

```go
YWJA
```

如果输入字符串不是 3 的倍数会怎样？在这种情况下，填充字符**=**将出现。

假设输入字符串有 4 个字符。

```go
ab@c
```

上述字符串的位表示为

```go
 a          b        @        c
01100001  01100010  01000000 01100011
```

前三个字节将被组合在一起。最后的字节将用 4 个额外的零进行填充，以使整体位数可被 6 整除。

```go
011000 010110 001001 000000 011000 110000
  24     22     9      0      24     48
```

使用上述数字在上表中进行索引。下面是映射。

| 24 | Y |
| --- | --- |
| 22 | W |
| 9 | J |
| 0 | A |
| 24 | Y |
| 48 | w |

这将变为

```go
YWJAYw==
```

每两个额外的零由=字符表示。由于我们添加了 4 个额外的零，因此在末尾有两个=。

现在让我们看另一个例子，其中输入字符串有 5 个字符。

```go
ab@cd
```

上述字符串的位表示为

```go
 a          b        @        c       d
01100001  01100010  01000000 01100011 01100100
```

前三个字节将组合在一起。最后两个字节将组合在一起，并用 2 个额外的零填充，以使总体位数能被 6 整除。

```go
011000 010110 001001 000000 011000 110110 010000
  24     22     9      0      24     54     16
```

使用上述数字在上表中索引。以下是映射。

| 24 | Y |
| --- | --- |
| 22 | W |
| 9 | J |
| 0 | A |
| 24 | Y |
| 54 | 2 |
| 16 | Q |

这将变成

```go
YWJAY2Q=
```

每两个额外的零由=字符表示。由于我们添加了 2 个额外的零，因此末尾有一个单独的=。另外，请注意，每个带填充的 Base64 编码字符串的长度都是 4 的倍数。

| **实际字符串** | **Base64** **编码** **字符串** | **长度** |
| --- | --- | --- |
| ab@ | YWJA | 4 |
| ab@c | YWJAYw== | 8 |
| ab@cd | YWJAY2Q= | 8 |

对于编码，我们只有上述讨论过的三种情况。

+   输入字符串的位数能被 6 整除。不添加填充。例如 ab@

+   输入字符串的位数不能被 6 整除，余数为 4\. 将添加双==填充。例如 ab@c

+   位数不能被 6 整除，余数为 2\. 将添加一个单个的=填充。例如 ab@cd

现在脑海中浮现的问题是，填充是否必要。答案是这要看情况。在我们看到解码如何工作后将进行讨论。

# **Base64 解码**

现在让我们将 Base64 编码的字符串解码为原始字符串。解码是编码的反向操作。让我们以这个例子为例。

```go
YWJAY2Q=
```

按每 4 个字符分组。

```go
YWJA 
```

和

```go
Y2Q=
```

现在从每个组中去掉尾随的=字符。对于剩下的字符串，将其转换为上表中的对应位表示。

```go
 Y      W       J      A     
011000 010110 001001 000000
```

和

```go
 Y      2      Q
011000 110110 010000
```

现在按 8 位分组。保留尾随的零。这是为了考虑添加的尾随=。尾随的零可以是 00 或 0000。

```go
01100001  01100010  01000000 
```

和

```go
01100011 01100100
```

现在对于上面的每个字节，根据 ASCII 表分配字符。

```go
 01100001  01100010  01000000
   a          b        @ 
```

和

```go
01100011 01100100
  c       d
```

因此最终字符串将是

```go
ab@cd
```

为什么我们在解码时将 Base64 编码的字符串分成 4 个字符一组？原因在于填充，这将在下一节中明确。

# **是否需要填充**

你可能在想，Base64 编码的填充=是否必要，因为我们在解码时简单地丢弃了填充。答案是这要看情况。

+   当你发送单个字符串时，填充不是必需的。

+   当你连接多个字符串的 Base64 编码时，填充是重要的。如果未填充的字符串被连接，则将无法得到原始字符串，因为关于添加的字节的信息将丢失。作为说明，请考虑下面的内容。

| **实际字符串** | **带填充的 Base64 编码** | **不带填充的 Base64 编码** |
| --- | --- | --- |
| a | YQ== | YQ |
| bc | YmM= | YmM |
| def | ZGVm | ZGVm |

现在我们来考虑两种情况。

**当连接的字符串未填充时**

在这种情况下，连接的 Base64 字符串将是

```go
YQYmMZGVm
```

尝试解码，你会得到下面的最终字符串，但这是不正确的。

```go
a&1
```

**当连接的字符串带填充时**

在这种情况下，连接的 Base64 字符串将是

```go
YQ==YmM=ZGVm
```

尝试按 4 个字符分组解码，你将得到最终的字符串如下，这是正确的。

```go
abcdef
```

现在再次出现的问题是，为什么需要连接多个 Base64 编码的字符串。答案是，在有流数据的情况下，想要在数据到达时发送 Base64 编码的数据总是好的。例如，视频缓冲。

所以，尽管在所有情况下并不是绝对必要，填充还是被鼓励。

# **大小**

基本上，Base64 在填充的情况下将 3 字节编码为 4 个 ASCII 字符。每个四个 ASCII 字符在网络上发送时将各占 1 字节。因此，结果大小总是比原始大小多 33.33%。所以如果字符串的原始大小为 n 字节，则 Base64 编码后的大小为：

```go
n*4/3
```

# **Base64 替代方案**

还有许多其他编码选项，但其中一些显著更难，而其他一些则占用太多空间。例如，Base 80 占用较少空间，但由于二进制的平方是自然基数，编码相对复杂。另外还有十六进制编码。它简单，但占用更多空间。

+   十六进制 – 其字符集为 16。

+   Base36 – 不区分大小写的编码

+   Base80

+   Base58

+   …

还有其他替代方案。

# **Base64 其他实现**

还有两个其他的 Base64 实现。

+   URL 中的 Base64。在此，**‘+’** 和 **‘\’** 符号分别被替换为 **‘+’** 和 **‘-‘**。这是因为 **‘+’** 和 **‘\’** 在 URL 编码中进一步被编码为十六进制序列，从而增加了 URL 的长度。例如，**‘+’** 会被转换为 ‘%2B’，**‘\’** 会在 URL 编码中被编码为 ‘%2F’。

+   文件名中的 Base64。在此 **‘\’** 被替换为 **‘-‘**。这是因为 **‘\’** 在 Unix 和 Windows 的文件路径中被使用。

# **Base64 应用程序**

+   电子邮件中的二进制数据传输，例如发送视频和音频作为电子邮件附件。

+   基本认证以 Base64 编码形式发送在 HTTP 协议中。

还有其他 Base64 应用程序。

# **结论**

这都是关于 Base64 编码的内容。希望你喜欢这篇文章。请在评论中分享反馈*
