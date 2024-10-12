<!--yml
category: 未分类
date: 2024-10-13 06:37:28
-->

# Golang Regex: Replace all string which matches a Regular Expression

> 来源：[https://golangbyexample.com/regex-replace-string-golang/](https://golangbyexample.com/regex-replace-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Replacement string as a literal string](#Replacement_string_as_a_literal_string "Replacement string as a literal string")
*   [A string having capturing group unnamed sub matches](#A_string_having_capturing_group_unnamed_sub_matches "A string having capturing group unnamed sub matches")
*   [A string having Capturing groups with named sub matches](#A_string_having_Capturing_groups_with_named_sub_matches "A string having Capturing groups with named sub matches")*  *## **Overview**

Golang **regexp** package provides a method which provides a method named **ReplaceAllString** which given a string can be used to replace all substring within that string that matches a regular expression.

[https://golang.org/pkg/regexp/#Regexp.ReplaceAllString](https://golang.org/pkg/regexp/#Regexp.ReplaceAllString)

Below is the signature of the method

```
func (re *Regexp) ReplaceAllString(src, repl string) string
```

It accepts two arguments

*   First is the input string

*   The second is the replacement string.

ReplaceAll returns a copy of the **src** string, replacing matches of the Regexp with the replacement string **repl**
The replacement string can be

*   A literal string

*   A string having capturing group unnamed sub matches.

*   A string having Capturing groups named sub matches

Sounds confusing? It will all be clear when we see examples of all. Let’s see them

## **Replacement string as a literal string**

The replacement string is a literal string in this case. Below is an example of the same. 

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`\w+:[0-9]\d{1,3}`)

	input := "The persons are John:21, Simon:23, and Mike:19"

	result := sampleRegexp.ReplaceAllString(input, "redacted")
	fmt.Println(string(result))
}
```

**Output**

```
The persons are redacted, redacted, and redacted
```

In the above example, we have below regex

```
`\w+:[0-9]\d{1,3}`
```

It matches the name and age pair of the form

```
name:age
```

Then we have below input string which has three **name:age** pairs

```
The persons are John:21, Simon:23, Mike:19
```

We redacted all the **name:age** pair by replacing it with the redacted keyword.

```
result := sampleRegexp.ReplaceAllString(input, "redacted")
```

Notice that the replacement string is a literal string that is

```
redacted
```

## **A string having capturing group unnamed sub matches**

This is the case when we have capturing group in the regex. To know the details about capturing group, please refer to this article

In the replacement string

*   **$1** or **${1}** represents the first sub match

*   **$2** or **${2}** represents the second sub match

*   …and so on

Let’s see the below example then it will be more clear

```
package main

import (
    "fmt"
    "regexp"
)

func main() {

    sampleRegexp := regexp.MustCompile(`(\w+):([0-9]\d{1,3})`)
    input := "The names are John:21, Simon:23, and Mike:19"

    result := sampleRegexp.ReplaceAllString(input, "$2:$1")
    fmt.Println(string(result))
}
```

**Output**

```
The names are 21:John, 23:Simon, and 19:Mike
```

In the above example, we have below regex

```
(\w+):([0-9]\d{1,3})
```

It matches the **name** and **age** pair same as above but it has two capturing groups (enclosed with parentheses) where **$1** captures name and **$2** captures age

*   **$0** or **${0}** – entire match

```
(\w+):([0-9]\d{1,3})
```

*   **$1** or **${1}** – first submatch

```
(\w+)
```

*   **$2** -or **${2}** – second submatch

```
([0-9]\d{1,3})
```

Then we have below input string which has three **name:age** pairs

```
The names are John:21, Simon:23, and Mike:19
```

Then in the replacement string, we swapped to have **age** first and then **name**

```
result := sampleRegexp.ReplaceAllString(input, "$2:$1")
```

That is why in the output we have **age** first and then **name**

```
The names are 21:John, 23:Simon, and 19:Mike
```

You can also replace only one of the sub-match. For example, if you want to redact age only then that can be done as well. See below program

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`(\w+):([0-9]\d{1,3})`)

	input := "The names are John:21, Simon:23, and Mike:19"

	result := sampleRegexp.ReplaceAllString(input, "$1")
	fmt.Println(string(result))
}
```

**Output**

```
The names are John, Simon, and Mike
```

In the above code, the replacement string is

```
$1
```

So it replaces the entire **name:age** pair with the only name.

You can also use **$0**. It will represent the entire match

```
package main

import (
    "fmt"
    "regexp"
)

func main() {
    sampleRegexp := regexp.MustCompile(`(\w+):([0-9]\d{1,3})`)
    input := "The names are John:21, Simon:23, and Mike:19"

    result := sampleRegexp.ReplaceAllString(input, "-$0-")
    fmt.Println(string(result))
}
```

**Output**

```
The names are -John:21-, -Simon:23-, and -Mike:19-
```

In the above example, we added **‘-‘** before and after **$0**.

```
-$0-
```

That is why the output is as above

In case the replacement string contains literal dollar then we can use the method **ReplaceAllLiteralString** method.

[https://golang.org/pkg/regexp/#Regexp.ReplaceAllLiteralString](https://golang.org/pkg/regexp/#Regexp.ReplaceAllLiteralString)

In this the replacement string is substituted directly without any kind of analysis on it i.e it is used literally. See below example

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`(\w+):([0-9]\d{1,3})`)

	input := "The names are John:21, Simon:23, and Mike:19"

	result := sampleRegexp.ReplaceAllLiteralString(input, "$1")
	fmt.Println(string(result))
}
```

**Output**

```
The names are $1, $1, and $1
```

As the output suggests above, it prints the literal dollar sign.

## **A string having Capturing groups with named sub matches**

This is the case when we have named capturing group in the regex. To know the details about capturing group, please refer to this article

Let’s see a program first in which we will see named capturing group. We will dissect this program to understand more clearly after that.

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`(?P<name>\w+):(?P<age>[0-9]\d{1,3})`)

	input := "The names are John:21, Simon:23, Mike:19"

	result := sampleRegexp.ReplaceAllString(input, "$Age:$Name")
	fmt.Println(string(result))
}</age></name>
```

**Output**

```
The names are 21:John, 23:Simon, 19:Mike
```

In the above example, we have below regex

```
`(?P<name>\w+):(?P<age>[0-9]\d{1,3})`</age></name>
```

It matches the name and age pair same as above but it has two name capturing groups

*   The first capturing group name is **“Name”**

```
(?P<name>\w+)</name>
```

*   The second capturing group name is **“Age”**

```
(?P<age>[0-9]\d{1,3})</age>
```

Then we have below input string which has three **name:age** pairs

```
The names are John:21, Simon:23 and Mike:19
```

Then in the replacement string, we swapped to have **age** first and then **name**

```
result := sampleRegexp.ReplaceAllString(input, "$Age:$Name")
```

That is why in the output we have **age** first and then **name**

```
The names are 21:John, 23:Simon, 19:Mike
```

This is all about replacing a string that matches a regular expression in golang. Hope you have liked this article. Please share feedback in the comments.

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)*