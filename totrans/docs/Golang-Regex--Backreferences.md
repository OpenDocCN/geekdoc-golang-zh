<!--yml
category: 未分类
date: 2024-10-13 06:37:33
-->

# Golang Regex: Backreferences

> 来源：[https://golangbyexample.com/golang-regex-backreferences/](https://golangbyexample.com/golang-regex-backreferences/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")
    *   [First Example](#First_Example "First Example")
    *   [Second Example](#Second_Example "Second Example")
*   [Replace Matched String](#Replace_Matched_String "Replace Matched String")*  *## **Overview**

Golang regex package [regexp](https://golang.org/pkg/regexp/) uses the [re2 engine](https://swtch.com/~rsc/regexp/regexp3.html) which doesn’t support backreferences. You can check the same here

[https://github.com/google/re2/wiki/Syntax](https://github.com/google/re2/wiki/Syntax)

It does mention that it doesn’t support backreferences.

**However, there is another golang package available that uses libpcre++, Perl regexes, and it supports backreferences.**

```
https://github.com/glenn-brown/golang-pkg-pcre/tree/master/src/pkg/pcre
```

## **Program**

So let’s see examples of backreferences in golang using this **pcre** package.

### **First Example**

Let’s say we want to match the repetition of a digit. Valid inputs are

```
1111
888888888
444
```

Regex to match for the same would be

```
(\d)\1+
```

Let’s dissect this regex

*   **(\d)** – Matches a single digit. The single-digit is enclosed in parentheses so it acts as a capturing group.

*   **\1** – Backreferences the first sub match by capturing group. So it will reference the first digit

*   **+** – One or more occurrences of the previous digit

Program for the same

```
package main

import (
	"fmt"

	"github.com/glenn-brown/golang-pkg-pcre/src/pkg/pcre"
)

func main() {
	regex := pcre.MustCompile(`(\d)\1+`, 0)

	matches := regex.MatcherString("1111", 0).Matches()
	fmt.Println("For 1111 : ", matches)

	matches = regex.MatcherString("88888888", 0).Matches()
	fmt.Println("For 88888888 : ", matches)

	matches = regex.MatcherString("444", 0).Matches()
	fmt.Println("For 444 : ", matches)

	matches = regex.MatcherString("123", 0).Matches()
	fmt.Println("For 123 : ", matches)
}
```

**Output**

```
For 1111 :  true
For 88888888 :  true
For 444 :  true
For 123 :  false
```

As expected it gives a match for repetition of digits

```
1111
888888888
444
```

And it does not match for below as it is not a repetition

```
123
```

### **Second Example**

Let’s say we want to match the repetition of a word separated by a colon. Valid inputs are

```
John:John
The names are Simon:Simon
```

Regex to match for the same would be

```
(\w+):\1
```

Let’s dissect this regex

*   **(\w+)** – Matches a word having more than one character. It is enclosed in parentheses so it acts as a capturing group.

*   **\1** – Backreferences the first sub match by capturing group. So it will reference the matched word

Program for the same

```
package main

import (
	"fmt"

	"github.com/glenn-brown/golang-pkg-pcre/src/pkg/pcre"
)

func main() {
	regex := pcre.MustCompile(`(\w+):\1`, 0)

	matches := regex.MatcherString("John:John", 0).Matches()
	fmt.Println("For John:John: ", matches)

	matches = regex.MatcherString("The names are Simon:Simon", 0).Matches()
	fmt.Println("For The names are Simon:Simon: ", matches)

	matches = regex.MatcherString("John:Simon", 0).Matches()
	fmt.Println("For John:Simon: ", matches)

}
```

**Output**

```
For John:John:  true
For The names are Simon:Simon:  true
For John:Simon:  false
```

As expected it gives a match for a string that contains a substring having a repetition of a word

```
John:John
The names are Simon:Simon
```

And it does not match for below as it does not contain a repetition of a word

```
John:Simon
```

## **Replace Matched String**

The **pcre** package also provides functionality to replace the matched string. Below is an example of the same.

```
package main

import (
	"fmt"

	"github.com/glenn-brown/golang-pkg-pcre/src/pkg/pcre"
)

func main() {
	regex := pcre.MustCompile(`(\d)\1+`, 0)

	input := "The number is 91-88888888"

	result := regex.ReplaceAll([]byte(input), []byte("redacted"), 0)
	fmt.Println("result: ", string(result))
}
```

**Output**

```
result:  The number is 91-redacted
```

In the above example, we have a regex with a backreference that matches a repetition of a digit. We then redact this repetition of digit using the **ReplaceAll** method of the **pcre** package

```
result := regex.ReplaceAll([]byte(input), []byte("redacted"), 0)
```

And as expected from the output, the repetition of the digit is correctly redacted

```
result:  The number is 91-redacted
```

Hope you have liked this tutorial. Please share the feedback in the comments

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [backreference](https://golangbyexample.com/tag/backreference/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [regex](https://golangbyexample.com/tag/regex/)*