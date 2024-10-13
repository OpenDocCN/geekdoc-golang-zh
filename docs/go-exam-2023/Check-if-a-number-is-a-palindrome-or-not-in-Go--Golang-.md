<!--yml

类别：未分类

日期：2024-10-13 06:42:24

-->

# 在Go（Golang）中检查一个数字是否是回文。

> 来源：[https://golangbyexample.com/check-number-palindrome-golang/](https://golangbyexample.com/check-number-palindrome-golang/)

目录

**   [概述](#Overview "概述")

+   [第一种解决方案 – 反转数字](#First_Solution_-_Reverse_the_Number "第一种解决方案 – 反转数字")

+   [第二种解决方案 – 使用递归](#Second_Solution_-_Use_Recursion "第二种解决方案 – 使用递归")*  *## **概述**

例如，下面的数字是回文。

```
1
121
12321
9
0
```

下面的数字不是回文。

```
-121
1211
```

我们有两种解决方案来判断一个数字是否是回文。

+   反转数字。如果反转后的数字等于原始数字，则该数字是回文。

+   另一种方法是使用递归并传入数字的指针。在递归树向下移动时，将数字除以10。当向上移动递归树时，将指针处的值除以10。在递归树的任何步骤中，当它们相遇时，原始数字的最后一位将是第一位，指针处数字的第一位将是最后一位。我们可以比较这两者以检查它们是否相等。这个检查在每次相遇时都会进行。

## **第一种解决方案 – 反转数字**

以下是相应的程序

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

**输出**

```
true
false
false
true
false
```

## **第二种解决方案 – 使用递归**

以下是相应的程序

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

**输出**

```
true
false
false
true
false
```*
