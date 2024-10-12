<!--yml
category: 未分类
date: 2024-10-13 06:42:24
-->

# Check if a number is a palindrome or not in Go (Golang)

> 来源：[https://golangbyexample.com/check-number-palindrome-golang/](https://golangbyexample.com/check-number-palindrome-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [First Solution – Reverse the Number](#First_Solution_-_Reverse_the_Number "First Solution – Reverse the Number")
*   [Second Solution – Use Recursion](#Second_Solution_-_Use_Recursion "Second Solution – Use Recursion")*  *## **Overview**

For eg below numbers are palindrome

```
1
121
12321
9
0
```

Below numbers are not

```
-121
1211
```

We have two solutions to find out if a number is palindrome or not

*   Reverse the number. If the reversed number is equal to the original number, then the number is a palindrome

*   The other approach is to use recursion and pass in the pointer to the number. While moving down the recursion tree divide the number by 10\. While moving up the recursion tree divide the value at the pointer to the number by 10\. During any step of the recursion tree when they meet the last digit in the original number will be the first and the last digit in the number at the pointer will be the first. We can compare these two to check if they are equal. This check is done each time they meet.

## **First Solution – Reverse the Number**

Below is the program for the same

```
package main

import (
	"fmt"
	"math"
)

func main() {
	output := isPalindrome(121)
	fmt.Println(output)

	output = isPalindrome(12)
	fmt.Println(output)

	output = isPalindrome(1234)
	fmt.Println(output)

	output = isPalindrome(12321)
	fmt.Println(output)

	output = isPalindrome(-101)
	fmt.Println(output)

}

func isPalindrome(x int) bool {

	if x < 0 {
		return false
	}
	if x < 10 {
		return true
	}
	xReversed := reverse(x)

	return xReversed == x
}

func reverse(x int) int {
	sign := "positive"
	if x >= 0 {
		sign = "positive"
	} else {
		sign = "negative"
	}

	x = int(math.Abs(float64(x)))

	var reversedDigit int

	for x > 0 {
		lastDigit := x % 10
		reversedDigit = reversedDigit*10 + lastDigit

		x = x / 10
	}

	if sign == "negative" {
		reversedDigit = reversedDigit * -1
	}

	return reversedDigit
}
```

**Output**

```
true
false
false
true
false
```

## **Second Solution – Use Recursion**

Below is the program for the same

```
package main

import "fmt"

func main() {
	a := 121
	output := isPalindrome(a, &a)
	fmt.Println(output)

	a = 12
	output = isPalindrome(a, &a)
	fmt.Println(output)

	a = 1234
	output = isPalindrome(a, &a)
	fmt.Println(output)

	a = 12321
	output = isPalindrome(a, &a)
	fmt.Println(output)

	a = -121
	output = isPalindrome(-a, &a)
	fmt.Println(output)

}

func isPalindrome(x int, dup *int) bool {
	if x < 0 {
		return false
	}
	if x < 10 {
		return true
	}

	palin := isPalindrome(x/10, dup)

	*dup = *dup / 10
	lastDigit := x % 10

	if palin && *dup%10 == lastDigit {
		return true
	}

	return false

}
```

**Output**

```
true
false
false
true
false
```*