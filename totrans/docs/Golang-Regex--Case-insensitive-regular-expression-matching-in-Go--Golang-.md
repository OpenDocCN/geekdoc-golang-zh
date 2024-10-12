<!--yml
category: 未分类
date: 2024-10-13 06:37:12
-->

# Golang Regex: Case insensitive regular expression matching in Go (Golang)

> 来源：[https://golangbyexample.com/case-insensitive-regex-golang/](https://golangbyexample.com/case-insensitive-regex-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The default behavior for regular expression matching in golang is case sensitive. But the default behavior can be changed by adding a set of flags to the beginning of the regular expression. The flag we need to add to the beginning of regex is:

```
(?i)
```

The flag **‘i’** is used to indicate that the regex will be case insensitive.

Here is the example regex for case sensitive and insensitive regex.

**Case sensitive regex**

```
abc
```

**Case insensitive regex**

```
(?i)abc
```

## **Program**

Let’s see a program for the same.

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegex := regexp.MustCompile("(?i)abc")

	match := sampleRegex.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)

	match = sampleRegex.Match([]byte("ABC"))
	fmt.Printf("For ABC: %t\n", match)
}
```

**Output**

```
For abc: true
For ABC: true
```

Notice the regex in the above program. We prefixed the regex with **(?i)** flag to indicate that this regex will be case insensitive

```
(?i)abc
```

As we can notice from the output, it gives a correct match for text **“abc”** as well as text **“ABC”**.

If we remove the prefix flag then it will  give afalse match for **“ABC”**

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegex := regexp.MustCompile("abc")

	match := sampleRegex.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)

	match = sampleRegex.Match([]byte("ABC"))
	fmt.Printf("For ABC: %t\n", match)
}
```

**Output**

```
For abc: true
For ABC: false
```

Notice the regex in the above program. We do not prefix the regex with **(?i)** flag. So it will fall back to the default behavior of being case sensitive.

```
abc
```

As we can notice from the output, it gives a correct match for text **“abc”** but it does not give a match for text **“ABC”**.

This is all about case insensitive regex matching in golang. Hope you have liked this article.

Please share the feedback in the comments.Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*