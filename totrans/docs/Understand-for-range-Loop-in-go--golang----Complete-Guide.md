<!--yml
category: 未分类
date: 2024-10-13 06:18:22
-->

# Understand for-range Loop in go (golang) – Complete Guide

> 来源：[https://golangbyexample.com/understand-for-range-loop-golang/](https://golangbyexample.com/understand-for-range-loop-golang/)

This is the  chapter 11 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [If Else](https://golangbyexample.com/understand-if-else-statement-golang/)
**Previous Tutorial** – [For Loop](https://golangbyexample.com/for-loop-in-golang/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

# **Overview**

When it comes to loop, golang has:

*   for loop
*   for-range loop

We saw for loop in the last tutorial. In this tutorial, we will be learning about the **for-range** loop only.

**for-range** loop is used to iterate over different collection data structures in golang such as

*   array or slice
*   string
*   maps
*   channel

Let’s see some examples now

# **Examples**

## **for-range loop for array/slice**

Here is the format of **for-range** when used with array/slice

```
for index, value := range array/slice {
    //Do something with index and value
}
```

This is how **for-range** loop works in case of array/slice. It iterates over the given array/slice starting from index zero and the body of the for range loop is executed for every value present at the index. Both index and value are optional in for-range when using with array/slice.

The below example shows how to use a **for-range** loop for a slice

*   With index and value
*   With value only
*   With index only
*   Without index and value

```
package main

import "fmt"

func main() {
    letters := []string{"a", "b", "c"}

    //With index and value
    fmt.Println("Both Index and Value")
    for i, letter := range letters {
        fmt.Printf("Index: %d Value:%s\n", i, letter)
    }

    //Only value
    fmt.Println("\nOnly value")
    for _, letter := range letters {
        fmt.Printf("Value: %s\n", letter)
    }

    //Only index
    fmt.Println("\nOnly Index")
    for i := range letters {
        fmt.Printf("Index: %d\n", i)
    }

    //Without index and value. Just print array values
    fmt.Println("\nWithout Index and Value")
    i := 0
    for range letters {
        fmt.Printf("Index: %d Value: %s\n", i, letters[i])
        i++
    }
}
```

**Output:**

```
Both Index and Value
Index: 0 Value:a
Index: 1 Value:b
Index: 2 Value:c

Only value
Value: a
Value: b
Value: c

Only Index
Index: 0
Index: 1
Index: 2

Without Index and Value
Index: 0 Value: a
Index: 1 Value: b
Index: 2 Value: c
```

## **for-range loop with a string**

In Golang string is a sequence of bytes. A string literal actually represents a UTF-8 sequence of bytes. In UTF-8, ASCII characters are single-byte corresponding to the first 128 Unicode characters. All other characters are between 1 -4 bytes. To understand it more consider the below string

```
sample := "a£c"
```

In above string

*   ‘a’ takes one byte as per UTF-8
*   ‘£’ takes two bytes as per UTF-8
*   ‘b’ takes one byte as per UTF-8

The above string has 1+2+1 = 4 bytes altogether. Therefore when we try to print the length of the string using the standard **len()** function it will output 4 and not 3 as **len()** function returns the number of bytes in the string.

```
fmt.Printf("Length is %d\n", len(sample))
```

Hence **standalone for** loop cannot be used to iterate over all characters of a string as it will iterate over bytes and not character. So below **for** loop will instead iterate four times and the print value corresponding to the byte present at that index.

```
 for i := 0; i < len(sample); i++ {
    fmt.Printf("%c\n", sample[i])
 }
```

It will output below string which is not same as **"a£c"** string

```
aÂ£b
```

The above output is not what we want. This is where **for-range** loop comes into picture for a string. It iterates over the Unicode points( also referred to as rune in golang) in a string and will correctly output  a, £, b.  Here is the format when using **for-range** with string

```
for index, character := range string {
    //Do something with index and character
}
```

Some point to note before we move to a code example

*   index is the starting point of the Unicode character in the string. For example in string "a£c" character "a" starts at index 0 , character "£" starts at index 1 while character "b" starts at index 3.
*   value is the Unicode point or basically each character in the string instead of bytes. It is also called rune. A rune in golang represents a Unicode Code Point
*   Both index and value are optional

Now let's see a code example

```
package main

import "fmt"

func main() {
    sample := "a£b"

    //With index and value
    fmt.Println("Both Index and Value")
    for i, letter := range sample {
        fmt.Printf("Start Index: %d Value:%s\n", i, string(letter))
    }

    //Only value
    fmt.Println("\nOnly value")
    for _, letter := range sample {
        fmt.Printf("Value:%s\n", string(letter))
    }

    //Only index
    fmt.Println("\nOnly Index")
    for i := range sample {
        fmt.Printf("Start Index: %d\n", i)
    }
}
```

**Output:**

```
Both Index and Value
Start Index: 0 Value:a
Start Index: 1 Value:£
Start Index: 3 Value:b

Only value
Value:a
Value:£
Value:b

Only Index
Start Index: 0
Start Index: 1
Start Index: 3
```

## **for-range loop with a map**

In case of map **for-range** iterates over key and values of a map. Below is the format for **for-range** when using with a map

```
for key, value := range map {
    //Do something with key and value
}
```

A point to be noted that both key and value are optional to be used while using **for-range** with maps. Let's see a simple code example

```
package main

import "fmt"

func main() {
    sample := map[string]string{
        "a": "x",
        "b": "y",
    }

    //Iterating over all keys and values
    fmt.Println("Both Key and Value")
    for k, v := range sample {
        fmt.Printf("key :%s value: %s\n", k, v)
    }

    //Iterating over only keys
    fmt.Println("\nOnly keys")
    for k := range sample {
        fmt.Printf("key :%s\n", k)
    }

    //Iterating over only values
    fmt.Println("\nOnly values")
    for _, v := range sample {
        fmt.Printf("value :%s\n", v)
    }
}
```

**Output**

```
Both Key and Value
key :a value: x
key :b value: y

Only keys
key :a
key :b

Only values
value :x
value :y
```

## **for-range loop with a channel**

**for-range** loop works differently too for a channel. For a channel, an index doesn't make any sense as the channel is similar to a pipeline where values enter from one and exit from the other end.

So in case of channel, the **for-range** loop will iterate over values currently present in the channel. After it has iterated over all the values currently present (if any), the **for-range** loop will not exit but instead wait for next value that might be pushed to the channel and it will exit only when the channel is closed

Below is the format when using **for-range** with channel

```
for value := range channel {
    //Do something value
}
```

Let's see a code example

```
package main

import "fmt"

func main() {
    ch := make(chan string)
    go pushToChannel(ch)
    for val := range ch {
        fmt.Println(val)
    }
}
func pushToChannel(ch chan<- string) {
    ch <- "a"
    ch <- "b"
    ch <- "c"
    close(ch)
}
```

**Output:**

```
a
b
c
```

# **Conclusion**

This is all about **for-range** loop in golang. Hope you like it. Please share feedback/improvements/mistakes in comments.

**Next Tutorial** – [If Else](https://golangbyexample.com/understand-if-else-statement-golang/)
**Previous Tutorial** – [For Loop](https://golangbyexample.com/for-loop-in-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)