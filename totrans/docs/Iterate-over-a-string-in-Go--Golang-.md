<!--yml
category: 未分类
date: 2024-10-13 06:20:46
-->

# Iterate over a string in Go (Golang)

> 来源：[https://golangbyexample.com/iterate-over-a-string-golang/](https://golangbyexample.com/iterate-over-a-string-golang/)

In Golang string is a sequence of bytes. A string literal actually represents a UTF-8 sequence of bytes. In UTF-8, ASCII characters are single-byte corresponding to the first 128 Unicode characters. All other characters are between 1 -4 bytes. To understand it more consider the below string

```
sameple := "a£c"
```

In above string

*   ‘a’ takes one byte as per UTF-8
*   ‘£’ takes two bytes as per UTF-8
*   ‘b’ takes one byte as per UTF-8

The above string has 1+2+1 = 4 bytes altogether. Therefore when we try to print the length of the string using the standard **len()** function it will output 4 and not 3 as **len()** function returns the number of bytes in the string.

```
fmt.Printf("Length is %d\n", len(sample))
```

Hence standalone for loop cannot be used to iterate over all characters of a string as it will iterate over bytes and not character. So below **for** loop will instead iterate four times and the print value corresponding to a byte at that index.

```
 for i := 0; i < len(sample); i++ {
    fmt.Printf("%c\n", sample[i])
 }
```

It will output below string which is not same as **sample** string

```
aÂ£b
```

Not we have mentioned the above limitation of using len() function and for loop, let's see two ways of calculating the length of the string.

*   Using for-range loop
*   By converting string to a rune array.

**Using for-range loop**

for-rage iterates over the Unicode points( also referred to as rune in golang) in a string and will correctly output  a, £, b. Hence it can also be used to calculate the length of the string. Here is the format when using for-range with string

```
for index, character := range string {
    //Do something with index and character
}
```

Sample code

```
package main

import "fmt"

func main() {
    sample := "a£b"

    for i, letter := range sample {
        fmt.Printf("Start Index: %d Value:%s\n", i, string(letter))
    }
}
```

**Output**

```
Start Index: 0 Value:a
Start Index: 1 Value:£
Start Index: 3 Value:b
```

**By converting string to rune array**

A rune represents a Unicode Point. By converting a string to rune array basically it is same as creating an array of Unicode Points of that string. Therefore once the string is converted into the rune array, it can be used to iterate over all characters of the string.

```
package main

import "fmt"

func main() {
    sample := "a£b"
    runeSample := []rune(sample)
    fmt.Printf("Length of given string is %d\n", len(runeSample))
    //Iterate
    for i := 0; i < len(runeSample); i++ {
        fmt.Println(string(runeSample[i]))
    }
}
```

**Output**

```
Length of given string is 3
a
£
b
```

*   [go](https://golangbyexample.com/tag/go/)*   [iterate](https://golangbyexample.com/tag/iterate/)*   [string](https://golangbyexample.com/tag/string/)