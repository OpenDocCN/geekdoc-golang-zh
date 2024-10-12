<!--yml
category: 未分类
date: 2024-10-13 06:40:57
-->

# Golang Regex: Understanding Curly Braces in Regular Expressions

> 来源：[https://golangbyexample.com/curly-braces-regex-golang/](https://golangbyexample.com/curly-braces-regex-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Examples](#Examples "Examples")
*   [Curly braces applied to a grouping](#Curly_braces_applied_to_a_grouping "Curly braces applied to a grouping")
*   [Curly Braces applied to a Character Class](#Curly_Braces_applied_to_a_Character_Class "Curly Braces applied to a Character Class")
*   [How to use the curly brace as a literal character in regex. ](#How_to_use_the_curly_brace_as_a_literal_character_in_regex "How to use the curly brace as a literal character in regex. ")*  *# **Overview**

Curly braces act as a repetition quantifier in regex. They specify the number of times a character before preceding it can appear in the input string or text. They can also be used to specify a range i.e specify the minimum and maximum of times a  character can appear. 

Its syntax is 

```
{min, max}
```

where 

*   **min** denotes the minimum number of times a character can appear

*   **max** denotes the maximum number of times a character can appear

For example

```
a{n}
```

This specifies that character “a” can appear exactly n times. Similarly for the below regex

```
\d{n}
```

This specifies that any digit can appear exactly n times. Curly braces can also be used to define a range.
For example

*   **{m,n}** – Atleast **m** and up to **n** times

*   **{m, }** – Atleast **m** times

*   **{, n}** – Upto **n** times

Let’s see an example for the same

# **Examples**

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`b{2}`)

	matches := sampleRegexp.FindString("bb")
	fmt.Println(matches)

	matches = sampleRegexp.FindString("bbb")
	fmt.Println(matches)

	matches = sampleRegexp.FindString("bbbb")
	fmt.Println(matches)
}
```

**Output**

```
bb
bb
bb
```

By default, curly braces are greedy or non-lazy. What does it mean? They will match all the possible characters and always prefers more. It is also possible to make the curly braces operator non-greedy or lazy. This can be done by adding a question mark after the curly braces operator. Let’s see an example for the same.

As you can see from the output that after adding a question mark operator after the curly brace operator, it tries to match the minimum number of characters as possible i.e it becomes nongreedy

That is why given regex

```
ab{2,4}
```

It gives a match **abb** for all below input strings

```
abb
abbb
abbbb
```

Program for the same

```
package main

import (
    "fmt"
    "regexp"
)

func main() {
    sampleRegexp := regexp.MustCompile(`ab{2,4}`)

    matches := sampleRegexp.FindStringSubmatch("abb")
    fmt.Println(matches)

    matches = sampleRegexp.FindStringSubmatch("abbb")
    fmt.Println(matches)

    matches = sampleRegexp.FindStringSubmatch("abbbb")
    fmt.Println(matches)
}
```

**Output**

```
abb
abbb
abbbb
```

while

**ab{2,4}?** will always give match as **abb** for all the above input strings

Program for the same

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`ab{2,4}?`)

	matches := sampleRegexp.FindString("abb")
	fmt.Println(matches)

	matches = sampleRegexp.FindString("abbb")
	fmt.Println(matches)

	matches = sampleRegexp.FindString("abbbb")
	fmt.Println(matches)
}
```

**Output**

```
abb
abb
abb
```

# **Curly braces applied to a grouping**

A part of the regular expression can be placed inside a balanced parenthesis. This part is one group now. We can additionally apply curly braces to this group. The curly braces will be added after the grouping 

Let’s see an example of the same.

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`(ab){2}`)

	matches := sampleRegexp.FindString("abab")
	fmt.Println(matches)

	matches = sampleRegexp.FindString("ababbc")
	fmt.Println(matches)
}
```

**Output**

```
abab
abab
```

# **Curly Braces applied to a Character Class**

Curly Brace quantifier can also be applied to the entire character class. Its meaning remains the same though. A character class is represented by square brackets in regex. Let’s see a program for the same.

We have below regex in the above program

```
[ab]{4}
```

It means it would match a string of length exactly 4 and comprised of characters **‘a’** and **‘b’** in any order

That is why the regex matches the below strings

```
abab
aaaa
bbbb
aabb
bbaa
```

And it does not match

```
aba - String of length 3
abbaa - String of length 5
```

# **How to use the curly brace as a literal character in regex. **

The escaping character can be placed before the opening brace or closing brace if they are needed to be used in a literal way.

A closing brace that is not preceded by an opening is treated as a literal closing brace. 

That is all about curly braces in regex in golang. Hope you have liked this article. Please share feedback in the comments

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*