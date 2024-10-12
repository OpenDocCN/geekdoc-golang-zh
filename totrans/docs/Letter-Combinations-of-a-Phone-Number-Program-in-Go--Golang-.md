<!--yml
category: 未分类
date: 2024-10-13 06:47:43
-->

# Letter Combinations of a Phone Number Program in Go (Golang)

> 来源：[https://golangbyexample.com/letter-combinations-phone-golang/](https://golangbyexample.com/letter-combinations-phone-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

An input string is given which consists of some digits. The mapping of digits to letters is similar to that of a phone keypad

```
2 = either "a", "b" or "c"
3 = either "d", "e" or "f"
4 = either "g", "h" or "i"
5 = either "j", "k" or "l"
6 = either "m", "n" or "co"
7 = either "p", "q" "r" or "s"
8 = either "t", "u" or "v"
9 = either "w", "x", "y" or "z"
```

So an input string is given which consists of only numbers. The objective is to return all letter combinations

Examples

```
Input: "3"
Output: [d e f]

Input: "34"
Output: [dg dh di eg eh ei fg fh fi]

Input: "345"
Output: [dgj dgk dgl dhj dhk dhl dij dik dil egj egk egl ehj ehk ehl eij eik eil fgj fgk fgl fhj fhk fhl fij fik fil]
```

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func letterCombinations(digits string) []string {
	if digits == "" {
		return nil
	}
	letterMap := make(map[string][]string)

	letterMap["2"] = []string{"a", "b", "c"}
	letterMap["3"] = []string{"d", "e", "f"}
	letterMap["4"] = []string{"g", "h", "i"}
	letterMap["5"] = []string{"j", "k", "l"}
	letterMap["6"] = []string{"m", "n", "o"}
	letterMap["7"] = []string{"p", "q", "r", "s"}
	letterMap["8"] = []string{"t", "u", "v"}
	letterMap["9"] = []string{"w", "x", "y", "z"}

	runeDigits := []rune(digits)
	length := len(runeDigits)

	temp := ""

	return letterCombinationsUtil(runeDigits, 0, length, temp, letterMap)

}

func letterCombinationsUtil(runeDigits []rune, start, length int, temp string, letterMap map[string][]string) []string {

	if start == length {
		return []string{temp}
	}

	currentDigit := string(runeDigits[start])

	letters := letterMap[currentDigit]

	final := make([]string, 0)
	for _, val := range letters {
		t := temp + val
		output := letterCombinationsUtil(runeDigits, start+1, length, t, letterMap)
		final = append(final, output...)
	}
	return final
}

func main() {

	output := letterCombinations("3")
	fmt.Println(output)

	output = letterCombinations("34")
	fmt.Println(output)

	output = letterCombinations("345")
	fmt.Println(output)
}
```

**Output**

```
[d e f]
[dg dh di eg eh ei fg fh fi]
[dgj dgk dgl dhj dhk dhl dij dik dil egj egk egl ehj ehk ehl eij eik eil fgj fgk fgl fhj fhk fhl fij fik fil]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*