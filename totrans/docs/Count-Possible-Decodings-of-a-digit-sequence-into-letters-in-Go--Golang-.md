<!--yml
category: 未分类
date: 2024-10-13 06:47:48
-->

# Count Possible Decodings of a digit sequence into letters in Go (Golang)

> 来源：[https://golangbyexample.com/count-possible-decodings-digit-golang/](https://golangbyexample.com/count-possible-decodings-digit-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Let’s say we have below number to letter mapping

```
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```

The objective is to count the number of possible decoding of a given digit sequence

Examples

```
Input: '15'
Output: 2

15 can be decoded as "AE" or "O"
```

## **Program**

Here is the program for the same.

```
package main

import (
	"fmt"
	"strconv"
)

func numDecodings(s string) int {

	runeArray := []rune(s)
	length := len(runeArray)
	if length == 0 {
		return 0
	}

	if string(runeArray[0]) == "0" {
		return 0
	}

	numwaysArray := make([]int, length)

	numwaysArray[0] = 1

	if length == 1 {
		return numwaysArray[0]
	}

	secondDigit := string(runeArray[0:2])
	num, _ := strconv.Atoi(secondDigit)
	if num > 10 && num <= 19 {
		numwaysArray[1] = 2
	} else if num > 20 && num <= 26 {
		numwaysArray[1] = 2
	} else if num == 10 || num == 20 {
		numwaysArray[1] = 1
	} else if num%10 == 0 {
		numwaysArray[1] = 0
	} else {
		numwaysArray[1] = 1
	}

	for i := 2; i < length; i++ {
		firstDigit := string(runeArray[i])
		if firstDigit != "0" {
			numwaysArray[i] = numwaysArray[i] + numwaysArray[i-1]
		}

		secondDigit := string(runeArray[i-1 : i+1])
		fmt.Println(i)
		fmt.Println(secondDigit)

		num, _ := strconv.Atoi(secondDigit)

		if num >= 10 && num <= 26 {
			numwaysArray[i] = numwaysArray[i] + numwaysArray[i-2]
		}
	}

	return numwaysArray[length-1]

}

func main() {
	output := numDecodings("15")
	fmt.Println(output)
}
```

**Output**

```
2
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*