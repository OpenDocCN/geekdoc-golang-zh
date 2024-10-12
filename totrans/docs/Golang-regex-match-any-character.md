<!--yml
category: 未分类
date: 2024-10-13 06:41:02
-->

# Golang regex match any character

> 来源：[https://golangbyexample.com/golang-regex-match-any-character/](https://golangbyexample.com/golang-regex-match-any-character/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

Dot ‘.’ character is one of the most commonly used metacharacters in the regular expression. It is used to match any character. By default, it doesn’t match a new line.

# **Program**

Now let’s see a simple program for Dot ‘.’ character

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(".")

	match := sampleRegexp.Match([]byte("a"))
	fmt.Printf("For a: %t\n", match)

	match = sampleRegexp.Match([]byte("b"))
	fmt.Printf("For b: %t\n", match)

	match = sampleRegexp.Match([]byte("ab"))
	fmt.Printf("For ab: %t\n", match)

	match = sampleRegexp.Match([]byte(""))
	fmt.Printf("For empty string: %t\n", match)
}
```

**Output**

```
For a: true
For b: true
For ab: true
For empty string: false
```

In the above program, we have a simple regex containing only one dot character.

```
sampleRegexp := regexp.MustCompile(".")
```

It matches below characters and string.

```
a
b
ab
```

It matches **ab** because by default the regex doesn’t do the match the full string unless we use the anchor characters (Caret and Dollar character). That is why it matches the first character ‘a’ in ‘ab’ and reports a match.

It doesn’t match an empty string.

Let’s see another example where we have two dots in the regex.

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("..")
	match := sampleRegexp.Match([]byte("ab"))
	fmt.Printf("For ab: %t\n", match)

	match = sampleRegexp.Match([]byte("ba"))
	fmt.Printf("For ba: %t\n", match)

	match = sampleRegexp.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)

	match = sampleRegexp.Match([]byte("a"))
	fmt.Printf("For a: %t\n", match)
}
```

**Output**

```
For ab: true
For ba: true
For abc: true
For a: false
```

In the above program, we have a simple regex containing two dots.

```
sampleRegexp := regexp.MustCompile("..")
```

It will match any given string which has at least two characters as a substring.

That is why it gives a match for

```
ab
ba
abc
```

and doesn’t give a match for

```
a
```

The dot **‘.’** as we mentioned before as well doesn’t match the new line. But the default behavior can be changed by adding a set of flags to the beginning of the regular expression. The flag we need to add to the beginning of regex is:

```
(?s)
```

Let’s see a program for the same

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(".")

	match := sampleRegexp.Match([]byte("\n"))
	fmt.Printf("For \\n: %t\n", match)

	sampleRegexp = regexp.MustCompile("(?s).")

	match = sampleRegexp.Match([]byte("\n"))
	fmt.Printf("For \\n: %t\n", match)
}
```

**Output**

```
For \n: false
For \n: true
```

```
sampleRegexp := regexp.MustCompile(".")
```

and

```
sampleRegexp = regexp.MustCompile("(?s).")
```

In the second regex, we have added the additional flag. That is why it gives a match for a new line while the first regex without a flag doesn’t give a match. 

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*