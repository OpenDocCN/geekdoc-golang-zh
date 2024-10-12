<!--yml
category: 未分类
date: 2024-10-13 06:38:13
-->

# Golang Regex: Match number or numeric digits in Regular Expression

> 来源：[https://golangbyexample.com/golang-regex-match-number/](https://golangbyexample.com/golang-regex-match-number/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Match a single digit](#Match_a_single_digit "Match a single digit")
*   [Match Repetition of digit](#Match_Repetition_of_digit "Match Repetition of digit")*  *## **Overview**

**\d** can be used to match digits in golang. In fact, **\d** can be used to match the entire range.

```
0-9
```

The regular expression for matching any digit will be

```
\d
```

In case you want to match only a particular digit, let’s say 5 then the regular expression will be that digit.

```
5
```

If you want to match two digits then below will be the regex

```
\d\d
```

## **Match a single digit**

Let’s see an example

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`\d`)

	fmt.Println("For regex \\d")
	match := sampleRegexp.MatchString("1")
	fmt.Printf("For 1: %t\n", match)

	match = sampleRegexp.MatchString("4")
	fmt.Printf("For 4: %t\n", match)

	match = sampleRegexp.MatchString("9")
	fmt.Printf("For 9: %t\n", match)

	match = sampleRegexp.MatchString("a")
	fmt.Printf("For a: %t\n", match)

	sampleRegexp = regexp.MustCompile(`5`)
	fmt.Println("\nFor regex 5")
	match = sampleRegexp.MatchString("5")
	fmt.Printf("For 5: %t\n", match)

	match = sampleRegexp.MatchString("6")
	fmt.Printf("For 6: %t\n", match)
}
```

**Output**

```
For regex \d
For 1: true
For 4: true
For 9: true
For a: false

For regex 5
For 5: true
For 6: false
```

In the above program, we have examples of  two regexes

*   **\d** – Match any digit

*   **5**  – Match only five

The first matches any single digit. That is why it matches

```
1
4
9
```

And it doesn’t match

```
a
```

The same is also evident from the output

The second regex matches only **“5”** and it doesn’t match **“6”** as is evident from the output

```
For regex 5
For 5: true
For 6: false
```

## **Match Repetition of digit**

Quantifiers can be used to match the repetition of digits. Example

*   **\d+** – Match one or more digits

*   **\d*** – Match zero or more digits

*   **\d{N}** – Match N number of digits

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`\d+`)

	fmt.Println(`For regex \d+`)
	match := sampleRegexp.MatchString("12345")
	fmt.Printf("For 12345: %t\n", match)

	match = sampleRegexp.MatchString("")
	fmt.Printf("For empty string: %t\n", match)

	sampleRegexp = regexp.MustCompile(`\d*`)

	fmt.Println()
	fmt.Println(`For regex \d*`)
	match = sampleRegexp.MatchString("12345")
	fmt.Printf("For 12345: %t\n", match)

	match = sampleRegexp.MatchString("")
	fmt.Printf("For empty string: %t\n", match)

	sampleRegexp = regexp.MustCompile(`\d{2}`)

	fmt.Println()
	fmt.Println(`For regex \d{2}`)
	match = sampleRegexp.MatchString("12")
	fmt.Printf("For 12: %t\n", match)

	match = sampleRegexp.MatchString("1")
	fmt.Printf("For 1: %t\n", match)
}
```

**Output**

```
For regex \d+
For 12345: true
For empty string: false

For regex \d*
For 12345: true
For empty string: true

For regex \d{2}
For 12: true
For 1: false
```

In the above program, we have examples of  three regexes

*   **\d+**

*   **\d***

*   **\d{N}**

**\d+** regex gives a match for **“12345”** but fails for an empty string

**\d***  gives a match for **“12345”** as well as an empty string

**\d{2}** matches a sequence of two digits. That is why it matches for **“12”** for fails for **“1”**

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)*