<!--yml
category: 未分类
date: 2024-10-13 06:44:04
-->

# Wildcard matching or regex matching program in Go (Golang)

> 来源：[https://golangbyexample.com/wildcard-matching-golang/](https://golangbyexample.com/wildcard-matching-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Recursive Solution](#Recursive_Solution "Recursive Solution")
*   [Dynamic Program Solution](#Dynamic_Program_Solution "Dynamic Program Solution")*  *## **Overview**

We are given an input regex and an input string. Regex can have two special characters

*   **Star Sign ‘*’ –** the star matches zero or more characters.

*   **Question Mark ‘?’ –** It matches any character.

The objective is to find whether the given input string matches the regex or not.

Eg

```
Input String: aa
Regex Sring: aa
Output: true

Input String: ab
Regex Sring: a?
Output: true

Input String: aaaa
Regex Sring: *
Output: true

Input String: aa
Regex Sring: a
Output: false
```

Below is the recursive solution for the same

## **Recursive Solution**

In the recursive solution

*   If we encounter a star * then we have two cases. We ignore the * character in the pattern and move on to the next character in the pattern. The other case is that we move one character in the input string assuming * matches at least one character. Basically check for a match with **(inputIndex, patternIndex+1)** and **(inputIndex+1, patternIndex)**. If either of them returns true then the input string matches the regex.

*   If we encounter a question mark ? then we simple proceed with **(inputIndex+1, patternIndex+1)**

*   If we encounter a simple character then we simply proceed in the input string as well as the patter  ie. we do proceed with **(inputIndex+1, patternIndex+1)**

Here is the program

```
package main

import "fmt"

func main() {
	output := isMatch("aa", "aa")
	fmt.Println(output)

	output = isMatch("aaaa", "*")
	fmt.Println(output)

	output = isMatch("ab", "a?")
	fmt.Println(output)

	output = isMatch("adceb", "*a*b")
	fmt.Println(output)

	output = isMatch("aa", "a")
	fmt.Println(output)

	output = isMatch("mississippi", "m??*ss*?i*pi")
	fmt.Println(output)

	output = isMatch("acdcb", "a*c?b")
	fmt.Println(output)
}

func isMatch(s string, p string) bool {
	runeInputArray := []rune(s)
	runePatternArray := []rune(p)
	if len(runeInputArray) > 0 && len(runePatternArray) > 0 {
		if runePatternArray[len(runePatternArray)-1] != '*' && runePatternArray[len(runePatternArray)-1] != '?' && runeInputArray[len(runeInputArray)-1] != runePatternArray[len(runePatternArray)-1] {
			return false
		}
	}
	return isMatchUtil([]rune(s), []rune(p), 0, 0, len([]rune(s)), len([]rune(p)))
}

func isMatchUtil(input, pattern []rune, inputIndex, patternIndex int, inputLength, patternLength int) bool {

	if inputIndex == inputLength && patternIndex == patternLength {
		return true
	} else if patternIndex == patternLength {
		return false
	} else if inputIndex == inputLength {
		if pattern[patternIndex] == '*' && restPatternStar(pattern, patternIndex+1, patternLength) {
			return true
		} else {
			return false
		}
	}

	if pattern[patternIndex] == '*' {
		return isMatchUtil(input, pattern, inputIndex, patternIndex+1, inputLength, patternLength) ||
			isMatchUtil(input, pattern, inputIndex+1, patternIndex, inputLength, patternLength)

	}

	if pattern[patternIndex] == '?' {
		return isMatchUtil(input, pattern, inputIndex+1, patternIndex+1, inputLength, patternLength)
	}

	if inputIndex < inputLength {
		if input[inputIndex] == pattern[patternIndex] {
			return isMatchUtil(input, pattern, inputIndex+1, patternIndex+1, inputLength, patternLength)
		} else {
			return false
		}
	}

	return false

}

func restPatternStar(pattern []rune, patternIndex int, patternLength int) bool {
	for patternIndex < patternLength {
		if pattern[patternIndex] != '*' {
			return false
		}
		patternIndex++
	}

	return true

}
```

**Output**

```
true
true
true
true
false
false
false
```

## **Dynamic Program Solution**

The above program is not an optimized solution as the subproblems are solved again and again. This problem can also be solved with DP.

Create a two-dimensional matrix named **isMatchingMatrix** where

**isMatchingMatrix[i][j]** will be true if the first **i** character in the input string matches the first **j** character in the pattern

```
If both input and pattern is empty
isMatchingMatrix[0][0] = true

If pattern is empty 
isMatchingMatrix[i][0] = fasle

If the input string is empty 
isMatchingMatrix[0][j] = isMatchingMatrix[0][j - 1] if pattern[j – 1] is '*'
```

Below is the program for the same.

```
package main

import "fmt"

func main() {
	output := isMatch("aa", "aa")
	fmt.Println(output)

	output = isMatch("aaaa", "*")
	fmt.Println(output)

	output = isMatch("ab", "a?")
	fmt.Println(output)

	output = isMatch("adceb", "*a*b")
	fmt.Println(output)

	output = isMatch("aa", "a")
	fmt.Println(output)

	output = isMatch("mississippi", "m??*ss*?i*pi")
	fmt.Println(output)

	output = isMatch("acdcb", "a*c?b")
	fmt.Println(output)
}

func isMatch(s string, p string) bool {

	runeInput := []rune(s)
	runePattern := []rune(p)

	lenInput := len(runeInput)
	lenPattern := len(runePattern)

	isMatchingMatrix := make([][]bool, lenInput+1)

	for i := range isMatchingMatrix {
		isMatchingMatrix[i] = make([]bool, lenPattern+1)
	}

	isMatchingMatrix[0][0] = true
	for i := 1; i < lenInput; i++ {
		isMatchingMatrix[i][0] = false
	}

	if lenPattern > 0 {
		if runePattern[0] == '*' {
			isMatchingMatrix[0][1] = true
		}
	}

	for j := 2; j <= lenPattern; j++ {
		if runePattern[j-1] == '*' {
			isMatchingMatrix[0][j] = isMatchingMatrix[0][j-1]
		}

	}

	for i := 1; i <= lenInput; i++ {
		for j := 1; j <= lenPattern; j++ {

			if runePattern[j-1] == '*' {
				isMatchingMatrix[i][j] = isMatchingMatrix[i-1][j] || isMatchingMatrix[i][j-1]
			}

			if runePattern[j-1] == '?' || runeInput[i-1] == runePattern[j-1] {
				isMatchingMatrix[i][j] = isMatchingMatrix[i-1][j-1]
			}
		}
	}

	return isMatchingMatrix[lenInput][lenPattern]
}
```

**Output**

```
true
true
true
true
false
false
false
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*