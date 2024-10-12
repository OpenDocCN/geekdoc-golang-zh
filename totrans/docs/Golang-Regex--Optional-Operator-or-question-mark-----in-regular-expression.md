<!--yml
category: 未分类
date: 2024-10-13 06:38:08
-->

# Golang Regex: Optional Operator or question mark (?) in regular expression

> 来源：[https://golangbyexample.com/optional-operator-regex-golang/](https://golangbyexample.com/optional-operator-regex-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")
*   [Question Mark Operator is non-lazy](#Question_Mark_Operator_is_non-lazy "Question Mark Operator is non-lazy")
*   [About the double question mark operator](#About_the_double_question_mark_operator "About the double question mark operator")
*   [Question Mark after quantifiers](#Question_Mark_after_quantifiers "Question Mark after quantifiers")*  *## **Overview**

Question Mark is the optional operator in regex. This means that it optionally matches the preceding character before the question mark

Eg. 

```
abcd?
```

This will match both **“abc”** and **“abcd”**.

## **Program**

Let’s see an example for the same.

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("abcd?")

	match := sampleRegexp.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)

	match = sampleRegexp.Match([]byte("abcd"))
	fmt.Printf("For abcd: %t\n", match)

}
```

**Output**

```
For abc: true
For abcd: true
```

Several characters can also be made optional by closing them with parentheses and then putting the question mark behind them. Eg

```
abc(de)?
```

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("abc(de)?")

	match := sampleRegexp.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)

	match = sampleRegexp.Match([]byte("abcde"))
	fmt.Printf("For abcde: %t\n", match)

	match = sampleRegexp.Match([]byte("abcd"))
	fmt.Printf("For abcd: %t\n", match)
}
```

**Output**

```
For abc: true
For abcde: true
For abcd: true
```

It matches **“abc”**  and **“abcde”**. 

It also matched **“abcd”**. You must be wondering why it matched **“abcd”**.

If the given string or text contains the regex as a substring then also it gives a match. That is why it gave a match as **“abcd”** contains **“abc”** as a substring which is a match for regex. If we want to do full string matches then we need to use the anchor characters at the start and end of the regex.  **Caret** anchor character will be used at the start and **Dollar** anchor character will be used at the end.

Let’s see an example for the same.

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("^abc(de)?$")

	match := sampleRegexp.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)

	match = sampleRegexp.Match([]byte("abcde"))
	fmt.Printf("For abcde: %t\n", match)

	match = sampleRegexp.Match([]byte("abcd"))
	fmt.Printf("For abcd: %t\n", match)
}
```

**Output**

```
For abc: true
For abcde: true
For abcd: false
```

## **Question Mark Operator is non-lazy**

Question Mark Operator is non-lazy or greedy. This means that it will match the optional pattern first. 

In the regular expression world, non-lazy(or sometimes also called greedy)  means trying to match as much as possible. While lazy(or sometimes also called non-greedy) means matching only as much as needed. 

For example for the given regex

```
https? 
```

If you try to match the below input string

```
Better is https
```

Then there are two options

*   Match **http**

*   Match **https**

Then it will always match **https** and never **http**. The reason is that it is non-lazy.  Even when it matches **http** it doesn’t stop and tries to match the optional character as well. If the optional character matches then it returns **https** otherwise it returns **http**. 

Let’s see an example for the same

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("https?")

	match := sampleRegexp.Find([]byte("Better is https"))
	fmt.Printf("Match: %s\n", match)
}
```

**Output**

```
Match: https
```

In the above program, we have used the **Find** function which returns the actual substring that matched with a regex. As you can notice in the output it matched **“https”** and not **“http”** because the question mark operator is non-lazy

## **About the double question mark operator**

It is lazy.  As soon it finds the first match, it does not try to match further. So for the above text, it will always give result as **“http”** and never **“https”**.

Let’s see an example

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("https??")

	match := sampleRegexp.Find([]byte("Better is https"))
	fmt.Printf("Match: %s\n", match)
}
```

**Output**

```
Match: http
```

## **Question Mark after quantifiers**

The question mark ‘?’ after quantifiers is lazy or non-greedy. The quantifiers could be

*   **Plus ‘+’** – One or more

*   **Asterisk ‘*’**  – Zero or more

See below example

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("http(s+?)")

	match := sampleRegexp.Find([]byte("Better is httpsss"))
	fmt.Printf("Match: %s\n", match)

	sampleRegexp = regexp.MustCompile("http(s*?)")

	match = sampleRegexp.Find([]byte("Better is httpsss"))
	fmt.Printf("Match: %s\n", match)
}
```

**Output**

```
Match: https
Match: http
```

In the above program, we have two cases

*   Question mark after a **plus** operator

*   Qusetion mark after **asterisk** operator

In both the case the input string is

```
Better is httpsss
```

In the first case, we used a question mark after the plus operator in the regex

```
"http(s+?)"
```

It gives below match as **“https”** instead of **“httpsss”** as question mark when used after the **Plus** operator is non-greedy

In the second case, we used a question mark after the asterisk sign in the regex

```
"http(s*?)"
```

It gives match as **“http”** instead of **“httpsss”** as the question mark when used after the **Asterisk** operator is non-greedy

Let’s see another example

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("(a+?)(a*)")

	match := sampleRegexp.FindStringSubmatch("aaaaaaa")
	fmt.Printf("Match: %s Length: %d\n", match, len(match))

	sampleRegexp = regexp.MustCompile("(a*?)(a*)")

	match = sampleRegexp.FindStringSubmatch("aaaaaaa")
	fmt.Printf("Match: %s Length: %d\n", match, len(match))
}
```

**Output**

```
Match: [aaaaaaa a aaaaaa] Length: 3
Match: [aaaaaaa  aaaaaaa] Length: 3
```

In the above program, again we have two cases

*   A question mark after a plus operator

*   A question mark after asterisk operator

In the first case, we have regex with two capture groups

```
(a+?)(a*)
```

The first capture group gives a single match of **‘a’** while the second capture group gives the rest. This show question mark operator used after the **Plus** operator is non-greedy or lazy

In the second case, we have regex again with two capture groups

```
(a*?)(a*)
```

The first capture group gives a zero match of **‘a’** while the second capture group gives the rest. This show question mark operator used after **Asterisk** operator is non-greedy or lazy

This is all about the question mark operator in Go. Hope you have liked this article. Please share feedback in the comments.

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [optional](https://golangbyexample.com/tag/optional/)*   [question](https://golangbyexample.com/tag/question/)*