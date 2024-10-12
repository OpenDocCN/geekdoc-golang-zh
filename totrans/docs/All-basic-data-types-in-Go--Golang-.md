<!--yml
category: 未分类
date: 2024-10-13 06:30:58
-->

# All basic data types in Go (Golang)

> 来源：[https://golangbyexample.com/all-basic-data-types-golang/](https://golangbyexample.com/all-basic-data-types-golang/)

This is the  chapter 7 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [Functions](https://golangbyexample.com/function-golang-complete-guide/)
**Previous Tutorial** –[Variables](https://golangbyexample.com/variables-in-golang-complete-guide/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Basic Types](#Basic_Types "Basic Types")
    *   [Integers ](#Integers "Integers ")
        *   [Signed Integers](#Signed_Integers "Signed Integers")
        *   [UnSigned](#UnSigned "UnSigned")
    *   [Floats](#Floats "Floats")
    *   [Complex Numbers](#Complex_Numbers "Complex Numbers")
    *   [Byte](#Byte "Byte")
    *   [Rune](#Rune "Rune")
    *   [String](#String "String")
    *   [Booleans](#Booleans "Booleans")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

Golang is a statically typed programming language meaning that each variable has a type. Go has several built-in types. Data types in Go can be categorized into two types.

1.  Basic Types
2.  Composite Types

*   Basic Types
    *   Integers
        *   Signed
            *   int
            *   int8
            *   int16 
            *   int32 
            *   int64
        *   Unsigned
            *   uint
            *   uint8
            *   uint16
            *   uint32
            *   uint64
            *   uintptr
    *   Floats
        *   float32
        *   float64
    *   Complex Numbers
        *   complex64
        *   complex128
    *   Byte
    *   Rune
    *   String
    *   Boolean
*   Composite Types
    *   Collection/Aggregation or Non-Reference Types
        *   Arrays
        *   Structs
    *   Reference Types
        *   Slices
        *   Maps
        *   Channels
        *   Pointers
        *   Function/Methods
    *   Interface
        *   Special case of empty Interface

# **Basic Types**

In this article we are going to discuss basic types only.

## **Integers** 

Integers can be signed or unsigned.

### **Signed Integers**

Signed integers are of 5 types as below

| **Type** | **Size** |
| int | Platform Dependent |
| int8 | 8 bits/1 byte |
| int16 | 16 bits/2 byte |
| int32 | 32 bits/4 byte |
| int64 | 64 bits/8 byte |

**int**

**Size:** Platform Dependent.

*   On 32 bit machines, the size of int will be 32 bits or 4 byte.
*   On 64 bit machines, the size of int will be 64 bits or 8 byte

**Range**: Again Platform dependent

*   On 32 bit machines, the size of int will be 32 bits or 4 bytes.
*   On 64 bit machines, the size of int will be 64 bits or 8 bytes

**When to Use:**

*   It is a good idea to use int whenever using signed Integer other than the cases mentioned below
    *   When the machine is a 32 bit and the range needed is greater than -231 to 231 -1, then use int64 instead of int. Note that in this case for int64,  2 32-bit memory addresses to form a 64-bit number together.
    *   When the range is less then use the appropriate integer type.

**Properties:**

*   Declare a int

```
var a int
```

*   int is default type for **integer.** When you don’t specify a type the default will be int

```
b := 2 //The default is also intfmt.Println(reflect.TypeOf(b)) => int
```

*   **bits** package of golang can help know the size of an **int** on your system

```
//This is computed as const uintSize = 32 << (^uint(0) >> 32 & 1) // 32 or 64sizeOfIntInBits := bits.UintSizefmt.Println(sizeOfIntInBits) => 32 0r 34
```

*   **unsafe.Sizeof()** function can also be used to see the size of int in bytes

**Full Working Code**

Below is the full working code of the above properties

```
package main

import (
    "fmt"
    "math/bits"
    "reflect"
    "unsafe"
)

func main() {
    //This is computed as const uintSize = 32 << (^uint(0) >> 32 & 1) // 32 or 64
    sizeOfIntInBits := bits.UintSize
    fmt.Printf("%d bits\n", sizeOfIntInBits)

    var a int
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))
    fmt.Printf("a's type is %s\n", reflect.TypeOf(a))

    b := 2
    fmt.Printf("b's typs is %s\n", reflect.TypeOf(b))
}
```

**Output:**

```
64 bits
8 bytes
a's type is int
b's typs is int
```

**int8**

**Size:** 8 bits or 1 byte

**Range**: -2⁷ to 2⁷ -1.

**When to Use:**

*   Use int8 when there it is known that the int range will be between -2⁷ to 2⁷ -1.  For temporary values such as loop invariants, it is still advisable to use int even though it might take more space because it is likely to be promoted to int in some operations or library calls.

*   For array values which lies between -27 to 27 -1, is a good use case for using int8\. For eg if you are storing ASCII index for lowercase letters then int8 can be used.
*   It is a good idea to use int8 for data values.

**int16**

**Size:** 16 bits or 2 byte
**Range**: -2^(15) to 2^(15) -1.**When to Use:**

*   Use int16 when there it is known that the int range will be between -2^(15) to 2^(15) -1.  For temporary values such as loop invariants, it is still advisable to use int even though it might take more space because it is likely to be promoted to int in some operations or library calls.

*   For array values which lie between -215 to 215 -1, is a good use case for using int8\. For eg if you are storing ASCII index for lowercase letters then int16 can be used.

**int32**

**Size:** 32 bits or 4 byte
**Range**: -2^(31) to 2^(31) -1.

**int64**

**Size:** 64 bits or 8 byte
**Range**: -2^(63) to 2^(63) -1.**When to Use:**

*   **int64** is used when range is higher. For eg **time.Duration** is of type **int64**

### **UnSigned**

UnSigned integers are of 5 types as below

| **Type** | **Size** |
| uint | Platform Dependent |
| uint8 | 8 bits/1 byte |
| uint16 | 16 bits/2 byte |
| uint32 | 32 bits/4 byte |
| uint64 | 64 bits/8 byte |

**uint**

**Size:** Platform Dependent.

*   On 32 bit machines the size of int will be 32 bits or 4 byte.
*   On 64 bit machines the size of int will be 64 bits or 8 byte

**Range**: Again Platform dependent

*   On 32 bit machines the range of int will be -2^(31) to 2^(31) -1.
*   On 64 bit machines the range of int will be -2^(63) to 2^(63) -1

**When to Use:**

*   It is a good idea to use uint  whenever using signed Integer other than the cases mention below
    *   When machine is a 32 bit and range needed is greater than -2^(31) to 2^(31) -1, then use **int64** instead **int**. Note that in this case for int64,  2 32-bit memory addresses to form a 64-bit number together.
    *   When the range is less then use the appropriate int type

**Properties:**

*   Declare a uint

```
var a uint
```

*   **bits** package of golang can help know the size of an **uint** on your system

```
//This is computed as const uintSize = 32 << (^uint(0) >> 32 & 1) // 32 or 64sizeOfUintInBits := bits.UintSizefmt.Println(sizeOfIntInBits) => 32 or 64
```

*   **unsafe.Sizeof()** function can also be used to see the size of uint in bytes

**Full Working Code**

Below is the full working code of the above properties

```
package main

import (
    "fmt"
    "math/bits"
    "reflect"
    "unsafe"
)

func main() {
    //This is computed as const uintSize = 32 << (^uuint(0) >> 32 & 1) // 32 or 64
    sizeOfuintInBits := bits.UintSize
    fmt.Printf("%d bits\n", sizeOfuintInBits)

    var a uint
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))
    fmt.Printf("a's type is %s\n", reflect.TypeOf(a))
}
```

**Output:**

```
64 bits
8 bytes
a's type is uint
```

**uintptr**

This is an unsigned integer type that is large enough to hold any pointer address. Therefore is size and range are platform dependent.

**Size:** Platform Dependent

*   On 32 bit machines, the size of an int will be 32 bits or 4 bytes.
*   On 64 bit machines, the size of an int will be 64 bits or 8 bytes.

**Range**: Again Platform dependent

*   On 32 bit machines the range of int will be -2^(31) to 2^(31) -1.
*   On 64 bit machines the range of int will be -2^(63) to 2^(63) -1

**Properties:**

*   A uintptr can be converted to unsafe.Pointer and vice versa
*   Arithmetic can be performed on the uintptr
*   uintptr even though it holds a pointer address, is just a value, and does not reference any object. Therefore
    *   Its value will not be updated if the corresponding object moves. For Eg When goroutine stack changes
    *   The corresponding object can be garbage collected.

****When to Use:****

 ***   Its purpose is to be used along with unsafe.Pointer mainly used for unsafe memory access.
*   When you want to save the pointer address value for printing it or storing it. Since the address is just stored and does not reference anything, the corresponding object can be garbage collected.

**Full Working Code**

```
package main

import (
    "fmt"
    "unsafe"
)

type sample struct {
    a int
    b string
}

func main() {
    s := &sample{a: 1, b: "test"}

   //Getting the address of field b in struct s
    p := unsafe.Pointer(uintptr(unsafe.Pointer(s)) + unsafe.Offsetof(s.b))

    //Typecasting it to a string pointer and printing the value of it
    fmt.Println(*(*string)(p))
}
```

**Output**

```
test
```

**uint8**

**Size:** 8 bits or 1 byte
**Range**:  0 to 255 or 0 to 2⁸ -1.**When to Use:**

*   Use uint8 when there it is known that the int range will be between 2⁸ -1.  For temporary values such as loop invariants, it is still advisable to use int even though it might take more space because it is likely to be promoted to int in some operations or library calls.
*   For array values which lies between  2⁸ -1\. is a good use case for using uint8\. For eg if you are storing ascii index in an array then **uint8** can be used.

**uint16**

**Size:** 16 bits or 2 byte
**Range**: 0 to 2^(16) -1**When to Use:**

*   Use int16 when there it is known that the int range will be between 0 to 2^(16) -1.  For temporary values such as loop invariants it is still advisable to use int even though it might take more space, because it is likely to be promoted to int in some operations or library calls.
*   For array values which lies between -0 to 2^(16) -1, is a good use case for using int8.

**uint32**

**Size:** 32 bits or 4 byte
**Range**: 0 to 2^(32) -1

**uint64**

**Size:** 64 bits or 8 byte
**Range**: 0 to 2^(64) -1**When to Use:**

*   uint64 is used when the range is higher.

## **Floats**

Floats are numbers with decimals. It is of two types

| **Type** | **Size** |
| float32 | 32 bits or 4 bytes |
| float64 | 64 bits or 8 bytes |

**float64** is the default float type. When you initialize a variable with a decimal value and don’t specify the float type, the default type inferred will be **float64**.

**float32**

**float32** uses a single-precision floating-point format to store values. Basically, it is the set of all IEEE-754 32-bit floating-point numbers. The 32 bits are divided into – 1 bit sign, 8 bits exponent, and 23 bits mantissa. float 32 take half much size as float 64 and are comparatively faster on some machine architectures.

**Size**: 32 bits or 4 bytes

**Range**: 1.2E-38 to 3.4E+38

**DefaultValue**: 0.0

**When to Use:**

If in your system memory is a bottleneck and range is less, then **float32** can be used.

**Example:**

The below code example illustrates below points

*   Declare a float32
*   Print size of float32 in bytes

**Code:**

```
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)

func main() {
    //Declare a float32
    var a float32 = 2

    //Size of float32 in bytes
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))
    fmt.Printf("a's type is %s\n", reflect.TypeOf(a))
}
```

**Output:**

```
4 bytes
a's type is float32
```

**float64**

float64 uses a double-precision floating-point format to store values. Basically it is the set of all IEEE-754 64-bit floating-point numbers. The 64 bits are divided into – 1-bit sign, 11 bits exponent, 52 bits mantissa. float64 takes twice as much size compared to float32 but can represent numbers more accurately than float32.

**Size**: 32 bits or 4 bytes

**Range**: 1.2E-38 to 3.4E+38

**DefaultValue**: 0.0

**When to Use:**

When the precision needed is high

**Example:**

The below code example illustrates below points

*   Declare a float64
*   Print size of float64 in bytes
*   Default is float64 when you don’t specify a type

**Code:**

```
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)

func main() {
    //Declare a float64
    var a float64 = 2

    //Size of float64 in bytes
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))
    fmt.Printf("a's type is %s\n", reflect.TypeOf(a))

    //Default is float64 when you don't specify a type
    b := 2.3
    fmt.Printf("b's type is %s\n", reflect.TypeOf(b))
}
```

**Output:**

```
8 bytes
a's type is float64
b's type is float64
```

## **Complex Numbers**

Complex Numbers are of two types

| **Type** | **Property** |
| complex64 | Both real and imaginary part are float32 |
| complex128 | Both real and imaginary part are float64 |

The default complex type is complex128

**Initialization**

Complex Numbers can be initialized in two ways

*   Using complex function. It has below signature. Do make sure that both a and b should be of same type , meaning either they both should be float32 or both should be float64

```
complext(a, b)
```

*   Using the shorthand syntax. This is used when creating a complex number with direct numbers. The complex type created using below method will be of type **complex128** if type is not specified

```
a := 5 + 6i
```

**complex64**

For complex 64 both real and imaginary part are float32

**Size**: Both real and imaginary part are of same size as float32\. It is of size 32 bits or 4 bytes

**Range**: Both real and imaginary part range is same as float32 i.e 1.2E-38 to 3.4E+38

**Example**

Below is a sample code that shows

*   How to create a complex64 number using the above two method
*   Print size of a complex64 number. Size will be  8 bytes(4 +4) which is equivalent to two float32 numbers
*   Print type of a complex64 number
*   + operation on complex number

**Code:**

```
package main
import (
    "fmt"
    "reflect"
    "unsafe"
)
func main() {
    var a float32 = 3
    var b float32 = 5

    //Initialize-1
    c := complex(a, b)

    //Initialize-2
    var d complex64
    d = 4 + 5i

    //Print Size
    fmt.Printf("c's size is %d bytes\n", unsafe.Sizeof(c))
    fmt.Printf("d's size is %d bytes\n", unsafe.Sizeof(d))

    //Print type
    fmt.Printf("c's type is %s\n", reflect.TypeOf(c))
    fmt.Printf("d's type is %s\n", reflect.TypeOf(d))

    //Operations on complex number
    fmt.Println(c+d, c-d, c*d, c/d)
}
```

**Output:**

```
c's size is 8 bytes
d's size is 8 bytes
c's type is complex64
d's type is complex64
(7+10i) (-1+0i) (-13+35i) (0.902439+0.12195122i)
```

**complex128**

For complex128 both **real** and **imaginary** part are **float64**

**Size**: Both **real** and **imaginary** part are of same size as float64\. It is of size 64 bits or 8 bytes

**Range**: Both **real** and **imaginary** part range is same as **float64** i.e -1.7E+308 to +1.7E+308

**Example**

Below is a sample code that shows

*   How to create a complex128 number using above two method. It also shows when type is not specified, the default type will be **complex128**
*   Print size of a complex128 number. Size will be  16 bytes(8 +8) which is equivalent to two float64 numbers
*   Print type of a complex128 number
*   Different operations on complex number

**Code:**

```
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)

func main() {
    var a float64 = 3
    var b float64 = 5

    //Initialize-1
    c := complex(a, b)

    //Initialize-2\. When don't specify a type , the default type will be complex128
    d := 4 + 5i

    //Print Size
    fmt.Printf("c's size is %d bytes\n", unsafe.Sizeof(c))
    fmt.Printf("d's size is %d bytes\n", unsafe.Sizeof(d))

    //Print type
    fmt.Printf("c's type is %s\n", reflect.TypeOf(c))
    fmt.Printf("d's type is %s\n", reflect.TypeOf(d))

    //Operations on complex number
    fmt.Println(c+d, c-d, c*d, c/d)
}
```

**Output:**

```
c's size is 16 bytes
d's size is 16 bytes
c's type is complex128
d's type is complex128
(7+10i) (-1+0i) (-13+35i) (0.902439024390244+0.12195121951219513i)
```

## **Byte**

byte in Go is an alias for **uint8** meaning it is an integer value. This integer value is of 8 bits and it represents one byte i.e number between 0-255). A single byte therefore can represent ASCII characters. Golang does not have any data type of ‘char’. Therefore

*   byte is used to represent the ASCII character

*   rune is used to represent all UNICODE characters which include every character that exists. We will study about rune later in this tutorial.

**Define Byte**

```
var rbyte byte := 'a'
```

While declaring byte we have specify the type, as we have in the program above. If we don’t specify the type, then the default type is meant as a **rune.**

**Example**

In below code example:

*   How to define a byte
*   Print the byte type
*   Print size of byte

```
package main
import (
    "fmt"
    "reflect"
    "unsafe"
)
func main() {
    var r byte = 'a'

    //Print Size
    fmt.Printf("Size: %d\n", unsafe.Sizeof(r))

    //Print Type
    fmt.Printf("Type: %s\n", reflect.TypeOf(r))

    //Print Character
    fmt.Printf("Character: %c\n", r)
    s := "abc"

    //This will the decimal value of byte
    fmt.Println([]byte(s))
}
```

**Output:**

```
Size: 1
Type: uint8
Character: a
[97 98 99]
```

## **Rune**

rune in Go is  an alias for **int32** meaning it is an integer value. This integer value is meant to represent a Unicode Code Point. To understand rune you have to know what Unicode is. Below is short description but you can refer to famous blog post about it – [The Absolute Minimum Every Software Developer Absolutely, Positively Must Know About Unicode and Character Sets (No Excuses!)](http://www.joelonsoftware.com/articles/Unicode.html)

**What is UniCode**

Unicode is a superset of ASCII characters which assigns a unique number to every character that exists. This unique number is called Unicode Code Point.

For eg

*   Digit **0** is represented as Unicode Point **U+0030 (Decimal Value – 48)**
*   Small Case **b** is represented as Unicode Point  **U+0062 (Decimal Value – 98)**
*   A pound symbol **£ **is represented as Unicode Point **U+00A3 (Decimal Value – 163)**

Visit [https://en.wikipedia.org/wiki/List_of_Unicode_characters](https://en.wikipedia.org/wiki/List_of_Unicode_characters) to know about Unicode Point of other characters. But Unicode doesn’t talk about how these code points will be saved in memory. This is where **utf-8** comes into picture

**UTF-8**

utf-8 saves every Unicode Point either using 1, 2, 3 or 4 bytes. ASCII points are stored using 1 byte. That is why rune is an alias for int32 because a Unicode Point can be of max 4 bytes in Go as in GO every string is encoded using utf-8.

Every rune is intended to refer to one Unicode Point.  For eg if you print a string after typecasting it to a rune array then it will print the Unicode Point for each of character. For for below string **“0b£”** output will be – **[U+0030 U+0062 U+00A3]**

```
fmt.Printf("%U\n", []rune("0b£"))
```

**Declare Rune**

A rune is declared using a character between single quotes like below declaring a variable named **‘rPound’**

```
rPound := '£'
```

After declaring Rune you can perform below things as well

*   **Print Type –** Output will be **int32**

```
fmt.Printf("Type: %s\n", reflect.TypeOf(rPound))
```

*   **Print Unicode Code Point –** Output will be **U+00A3**

```
fmt.Printf("Unicode CodePoint: %U\n", rPound)
```

*   **Print Character –** Output will be **£**

```
fmt.Printf("Character: %c\n", r)
```

**When to Use**

You should use a rune when you intend to save Unicode Code Point in the value. A rune array should be used when all values in the array are meant to be a Unicode Code Point.

**Code:**

Below is the code illustrating each point we discussed

```
package main
import (
    "fmt"
    "reflect"
    "unsafe"
)
func main() {
    r := 'a'

    //Print Size
    fmt.Printf("Size: %d\n", unsafe.Sizeof(r))

    //Print Type
    fmt.Printf("Type: %s\n", reflect.TypeOf(r))

    //Print Code Point
    fmt.Printf("Unicode CodePoint: %U\n", r)

    //Print Character
    fmt.Printf("Character: %c\n", r)
    s := "0b£"

    //This will print the Unicode Points
    fmt.Printf("%U\n", []rune(s))

    //This will the decimal value of Unicode Code Point
    fmt.Println([]rune(s))
}
```

**Output:**

```
Size: 4
Type: int32
Unicode CodePoint: U+0061
Character: a
[U+0030 U+0062 U+00A3]
[48 98 163]
```

## **String**

string is a read only slice of bytes in golang. String can be initialized in two ways

*   using double quotes “” eg “this”

string in double quotes honors the escape sequences. For eg if the string contains a \n then while printing there will be a new line

*   using back quotes ` eg  \`this`

String in back quotes is just a raw string and it does not honor any kind of escape sequences.

Each character in a string will occupy some bytes depending upon encoding used. For eg in utf-8 encoded string, each character will occupy between 1-4 bytes. You can read about utf-8 in this must read famous blog-[The Absolute Minimum Every Software Developer Absolutely, Positively Must Know About Unicode and Character Sets (No Excuses!)](http://www.joelonsoftware.com/articles/Unicode.html).   In utf-8 , the characters **a** or **b** are encoded using 1  byte while the character pound sign **£** is encoded using two bytes . Therefore the string “ab£” will output 4 bytes when you will convert the string to byte array and print it like below

```
s := "ab£"
fmt.Println([]byte(s))
```

**Output**

```
[48 98 194 163]
```

Also when you try to print the length of the above string using **len(“ab£”),** it will output 4 and not 3 because it contains 4 bytes.

Also note that **range** loops over sequences of byte which form each character, therefore for the below range loop

```
for _, c := range s {
   fmt.Println(string(c))
}
```

Output will be

```
a
b
£
```

There are many operations that can be performed on a string. One such operation is concatenation which combines two string. The sign ‘+’ is used for concatenation. Let’s see full working  code for all above things that we discussed

**Code:**

```
package main
import (
    "fmt"
)
func main() {
    //String in double quotes
    x := "this\nthat"
    fmt.Printf("x is: %s\n", x)

    //String in back quotes
    y := `this\nthat`
    fmt.Printf("y is: %s\n", y)
    s := "ab£"

    //This will print the byte sequence. 
    //Since character a and b occupies 1 byte each and £ character occupies 2 bytes. 
    //The final output will 4 bytes
    fmt.Println([]byte(s))

    //The output will be 4 for same reason as above
    fmt.Println(len(s))

    //range loops over sequences of byte which form each character
    for _, c := range s {
        fmt.Println(string(c))
    }

    //Concatenation
    fmt.Println("c" + "d")
}
```

**Output:**

```
x is: this
that
y is: this\nthat
[97 98 194 163]
4
a
b
£
cd
```

## **Booleans**

The data type is **bool** and has two possible values true or false.

Default Value: false

Operations:

*   AND – &&
*   OR  – ||
*   Negation – !

**Example**

The below code example shows

*   If not initialized the default value is **false**
*   All the above operations on the bool

**Code**

```
package main

import "fmt"

func main() {
    //Default value will be false it not initialized
    var a bool
    fmt.Printf("a's value is %t\n", a)

    //And operation on one true and other false
    andOperation := 1 < 2 && 1 > 3
    fmt.Printf("Ouput of AND operation on one true and other false %t\n", andOperation)

    //OR operation on one true and other false
    orOperation := 1 < 2 || 1 > 3
    fmt.Printf("Ouput of OR operation on one true and other false: %t\n", orOperation)

    //Negation Operation on a false value
    negationOperation := !(1 > 2)
    fmt.Printf("Ouput of NEGATION operation on false value: %t\n", negationOperation)
}
```

**Output:**

```
a's value is false
Ouput of AND operation on one true and other false false
Ouput of OR operation on one true and other false: true
Ouput of NEGATION operation on false value: true
```

# Conclusion

This is all about basic types in golang. Hope you have liked this article. Please share feedback or improvement or mistakes in comments

**Next Tutorial** – [Functions](https://golangbyexample.com/function-golang-complete-guide/)
**Previous Tutorial** –[Variables](https://golangbyexample.com/variables-in-golang-complete-guide/)***