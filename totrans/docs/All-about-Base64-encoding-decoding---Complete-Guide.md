<!--yml
category: 未分类
date: 2024-10-13 06:33:36
-->

# All about Base64 encoding/decoding – Complete Guide

> 来源：[https://golangbyexample.com/base64-complete-guide/](https://golangbyexample.com/base64-complete-guide/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Why Base64 encoding is needed](#Why_Base64_encoding_is_needed "Why Base64 encoding is needed")
*   [How Base64 encoding/decoding works](#How_Base64_encodingdecoding_works "How Base64 encoding/decoding works")
*   [Base64 Decoding](#Base64_Decoding "Base64 Decoding")
*   [Is Padding Necessary](#Is_Padding_Necessary "Is Padding Necessary")
*   [Size](#Size "Size")
*   [Base64 Alternatives](#Base64_Alternatives "Base64 Alternatives")
*   [Base64 Other Implementations](#Base64_Other_Implementations "Base64 Other Implementations")
*   [Base64 Applications](#Base64_Applications "Base64 Applications")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

Base64 is also known as Base64 Content-Transfer-Encoding. Base64 is the encoding of binary data into ASCII text. But it uses only 64 characters and plus one padding character which are present in the most character set. So it is a way of representing binary data using only printable characters.  These printable characters are

*   Lowercase Alphabets **a-z**
*   UpperCase Alphabets **A-Z**
*   Numbers **0-9**
*   Characters **+** an **/**
*   **=** is used as a padding character

Base64 encoding is used for transferring data over a medium that doesn’t handle binary data correctly. So Base64 encoding of data is done to make sure that the data remains intact without any modification through this media. A base 64 encoding will look like as below

```
OWRjNGJjMDY5MzVmNGViMGExZTdkMzNjOGMwZTI3ZWI==
```

# **Why Base64 encoding is needed**

Let’s talk about history to know why base64 was needed.  Initially, when the mailing system was started it was only text which was transferred through email. Later on, the email also started supporting attachments which including video and audio. Video and audio are binary data and when binary data is transferred over the internet there are good chances of it being corrupt. The question is why. Some media such as email support only textual data. They are only meant for streaming text and as such these protocols might interpret your binary data as control characters or some special characters in the binary data may be interpreted differently. The problem with binary data is that some characters in the binary data might have special meaning in some media. In other words, these media are not 8 bit clean and are only meant to handle textual data.

So base64 was meant for media that does not support binary data and which work with only textual characters. As it is guaranteed that data won’t be corrupted. It is mostly needed for legacy systems that work with only ASCII data.

But why 64 characters and not more. These 64 characters are present in most character sets so you can be reasonably confident that your data will the other end without any corruption.

Note that it is encoding and not encryption. What’s the difference between encoding and encryption?

**Encoding**

Encoding means transforming data into another format so that it can be properly consumed by a system that only understands the converted format. Encoding is just an algorithm that is publicly available. There is no secret key involved and is reversible. So anyone knowing the algorithm can simply reverse and get the original data. Now the question is why it is needed. Some of the transfer media only understand textual data or a certain number of characters. Your binary data cannot be transferred through such a medium as there are chances of data being getting corrupted.

Encoding doesn’t provide any kind of security, instead, it is only meant for compatibility across different media which understand different format

**Encryption**

Encryption is meant for confidentiality. It is always protected by a key and it meant to be decrypted by anyone who knows the key. So in the case of encryption, the data can only be reversed by someone who possesses the key.

For eg sending a password over the network. In this case, we encrypt the password so that any unintended person cannot read it. Example HTTPS

So essentially encoding is meant for compatibility and not for encryption.

# **How Base64 encoding/decoding works**

Base64 encoding is described in [https://tools.ietf.org/html/rfc4648](https://tools.ietf.org/html/rfc4648)

Base64 encoding will translate every 3 bytes of data into 4 encoded characters. It will start scanning from left to right, and then it will select the first 3 bytes of data representing 3 characters. These 3 bytes will be 24 bits. Now it will divide these 24 bits into four parts of 6 bits each. Then each 6-bit group will be indexed in the below table to get the mapped character.

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

Let’s understand it with an example. Let’s say we have the below string

```
ab@
```

The bits representation of the above string will be

```
 a          b        @
01100001  01100010  01000000
```

These total 24 bits will be grouped into 4 groups of 6 bits each

```
011000 010110 001001 000000
```

The numeric representation of the above bits is

```
 24    22     9      0
011000 010110 001001 000000
```

Use the above numbers to index into the base64 table. Below will the mapping

| 24 | Y |
| 22 | W |
| 9 | J |
| 0 | A |

So the base64 encoded string will be

```
YWJA
```

What if the input string is not in multiples of 3? In this case padding character **=** will come in the picture

Let’s say the input string has 4 characters

```
ab@c
```

The bits representation of the above string will be

```
 a          b        @        c
01100001  01100010  01000000 01100011
```

The first three bytes will be group together. The last bytes will be padded with 4 extra zero to make the overall bits divisible by 6. 

```
011000 010110 001001 000000 011000 110000
  24     22     9      0      24     48
```

Use the above numbers to index into the above table. Below will the mapping

| 24 | Y |
| 22 | W |
| 9 | J |
| 0 | A |
| 24 | Y |
| 48 | w |

This will become

```
YWJAYw==
```

Each two extra zero is represented by = character. And since we added 4 extra zero, Hence two = at the end.

Now let’s see another example in which the input string has 5 characters

```
ab@cd
```

The bits representation of the above string will be

```
 a          b        @        c       d
01100001  01100010  01000000 01100011 01100100
```

The first three bytes will be group together. The last two bytes will be grouped together and padded with 2 extra zero to make the overall bits divisible by 6. 

```
011000 010110 001001 000000 011000 110110 010000
  24     22     9      0      24     54     16
```

Use the above numbers to index into the above table. Below will the mapping

| 24 | Y |
| 22 | W |
| 9 | J |
| 0 | A |
| 24 | Y |
| 54 | 2 |
| 16 | Q |

This will become

```
YWJAY2Q=
```

Each two extra zero is represented by = character. And since we added 2 extra zero, Hence one single = at the end. Also, note that each of the base encoded string length with padding is multiple of four

| **Actual String** | **Base64** **Encoded** **String** | **Length** |
| ab@ | YWJA | 4 |
| ab@c | YWJAYw== | 8 |
| ab@cd | YWJAY2Q= | 8 |

For encoding, we have only three cases that we had discussed above

*   The number of bits in the input string is divisible by 6\. No padding is added. Eg ab@

*   The number of bits in the input string is not divisible by 6 and the remainder is 4\. A double == padding will be added.Eg ab@c

*   The number of bits is not divisible 6 by and the remainder is 2\. A single = padding will be added.  Eg ab@cd

Now the question which comes to mind is whether padding necessary. The answer is that it depends. We will discuss it after we have seen how decoding works.

# **Base64 Decoding**

Let’s now decode the base64 encoded string into the original string. Decoding is the opposite of encoding. Let’s take the example

```
YWJAY2Q=
```

Group into multiple of 4 characters each.

```
YWJA 
```

and

```
Y2Q=
```

Now remove the trailing = characters from each of the groups. For the remaining string convert it into their corresponding bit representation from the above table.

```
 Y      W       J      A     
011000 010110 001001 000000
```

and

```
 Y      2      Q
011000 110110 010000
```

Now do grouping into a group of 8 bits. Leave the trailing zeros. This is to account for trailing = added. The trailing zero will be either 00 or 0000

```
01100001  01100010  01000000 
```

and

```
01100011 01100100
```

Now for each of the byte above, assign the character as per the ASCII table

```
 01100001  01100010  01000000
   a          b        @ 
```

and

```
01100011 01100100
  c       d
```

Hence the final string will be

```
ab@cd
```

Why did we group the Base64 encoded string into groups of 4 for decoding? The reason is because of padding and it will get clear in the next section

# **Is Padding Necessary**

You might be wondering now whether the padding of = is necessary for base64 encoding or not, since we simply discarded the padding while decoding. The answer is that it depends

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

So that is why padding is encouraged though not absolutely not necessary in all cases.

# **Size**

Since essentially Base64 encodes 3 bytes to 4 ASCII characters in case of padding. Each of the four ASCII characters will send as 1 byte each over the network. Hence the resultant size will always be 33.33% more than the original. So if the original size of the string  is n bytes then size, after base64 encode, will be

```
n*4/3
```

# **Base64 Alternatives**

There are many other options available for encoding as well, but some of them are significantly harder while others take too much space. For example, there is Base 80 which takes less space but is significantly harder as anything to the power two are natural bases for binary. Also there is hexadecimal encoding. It is simple but takes more space.

*   Hexadecimal – It has a character set of 16

*   Base36 – Case insensitive encoding

*   Base80

*   Base58

*   …

There are other alternatives as well.

# **Base64 Other Implementations**

Two other base64 implementations exist

*   Base64 for URL. In this, the **‘+’** and **‘\’** sign is replaced with **‘+’** and **‘-‘**. This is because **‘+’** and **‘\’**  are further encoded into hexadecimal sequences due to URL encoding thereby further increasing the length of the URL. For eg **‘+’** will be converted to ‘%2B’ and **‘\’** will be encoded to ‘%2F’ in the URL encoding.

*   Base64 for filenames. In this ‘\’ is replaced with **‘-‘**. This is because the **‘\’** is used in file paths in both Unix and Windows.

# **Base64 Applications**

*   Binary data transfer in emails for eg sending video and audio as email attachments

*   Basic Auth is sent as Base64 encoded in the HTTP protocol

There are other Base64 applications as well

# **Conclusion**

This is all about Base64 encoding. Hope you have liked this article. Please share feedback in the comments*