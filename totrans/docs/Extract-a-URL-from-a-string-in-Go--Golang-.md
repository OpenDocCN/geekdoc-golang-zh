<!--yml
category: 未分类
date: 2024-10-13 06:40:26
-->

# Extract a URL from a string in Go (Golang)

> 来源：[https://golangbyexample.com/extract-url-golang/](https://golangbyexample.com/extract-url-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

Below go package can be used to extract URL from a given string

[https://github.com/mvdan/xurls](https://github.com/mvdan/xurls)

There are two ways of using this package

*   **Strict** – In strict mode, it matches only URLs with a scheme

*   **Relaxed** – In relaxed mode, it matches any URL matched by strict plus any URLs without a scheme

You can specify the scheme to filter on. There is a function for that

**StrictMatchingScheme**

# **Program**

Let’s first see a program

```
package main

import (
	"fmt"

	"mvdan.cc/xurls/v2"
)

func main() {
	xurlsStrict := xurls.Strict()
	output := xurlsStrict.FindAllString("golangbyexample.com is https://golangbyexample.com", -1)
	fmt.Println(output)

	xurlsRelaxed := xurls.Relaxed()
	output = xurlsRelaxed.FindAllString("The website is golangbyexample.com", -1)
	fmt.Println(output)

	output = xurlsRelaxed.FindAllString("golangbyexample.com is https://golangbyexample.com", -1)
	fmt.Println(output)
}
```

**Output**

```
[https://golangbyexample.com]
[golangbyexample.com]
[golangbyexample.com https://golangbyexample.com]
```

Notice in the strict mode, it does not return the [golangbyexample.com](http://golangbyexample.com) in the output as that is without a scheme.

Let’s see another program to extract multiple URLs

```
package main

import (
	"fmt"

	"mvdan.cc/xurls/v2"
)

func main() {
	xurlsStrict := xurls.Strict()
	input := "The webiste is https://golangbyexample.com:8000/tutorials/intro amd mail to mailto:contactus@golangbyexample.com"
	output := xurlsStrict.FindAllString(input, -1)
	fmt.Println(output)
}
```

**Output**

```
[https://golangbyexample.com:8000/tutorials/intro mailto:contactus@golangbyexample.com]
```

If we want to restrict the output to a particular scheme then that also can be done.

```
package main

import (
	"fmt"
	"log"

	"mvdan.cc/xurls/v2"
)

func main() {
	xurlsStrict, err := xurls.StrictMatchingScheme("https")
	if err != nil {
		log.Fatalf("Some error occured. Error: %s", err)
	}
	input := "The webiste is https://golangbyexample.com:8000/tutorials/intro amd mail to mailto:contactus@golangbyexample.com"
	output := xurlsStrict.FindAllString(input, -1)
	fmt.Println(output)
}
```

**Output**

```
[https://golangbyexample.com:8000/tutorials/intro]
```

In the above program, we provided the scheme as https which is why we have only one output

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*