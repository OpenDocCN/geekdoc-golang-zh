<!--yml
category: 未分类
date: 2024-10-13 06:25:25
-->

# Error in Go (Golang)- Advanced

> 来源：[https://golangbyexample.com/error-in-golang-advanced/](https://golangbyexample.com/error-in-golang-advanced/)

This is the  chapter 27 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [Panic and Recover](https://golangbyexample.com/panic-and-recover-golang/)
**Previous Tutorial** – [Error – Part 1](https://golangbyexample.com/error-in-golang/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Wrapping of error](#Wrapping_of_error "Wrapping of error")
*   [Unwrap an error](#Unwrap_an_error "Unwrap an error")
*   [Check if two error are equal](#Check_if_two_error_are_equal "Check if two error are equal")
    *   [Using the equality operator (==)](#Using_the_equality_operator "Using the equality operator (==)")
    *   [Using the Is function of errors package](#Using_the_Is_function_of_errors_package "Using the Is function of errors package")
*   [Get the underlying error from an error represented by the error interface](#Get_the_underlying_error_from_an_error_represented_by_the_error_interface "Get the underlying error from an error represented by the error interface")
    *   [Using the .({type}) assert](#Using_the_type_assert "Using the .({type}) assert")
    *   [Using the As function of errors package](#Using_the_As_function_of_errors_package "Using the As function of errors package")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

In the article we will be covering the advanced topics related to error in go.

*   Wrapping and un-wrapping errors

*   Error comparison 

*   Extract underlying type from error

*   **As** and **Is** function of errors package

Please refer to link below first which starts with basics of **error** in go.

[https://golangbyexample.com/error-in-golang/](https://golangbyexample.com/error-in-golang/)

That article will cover basic things about error such as:

*   Overview of error

*   Error interface

*   Different ways of creating an error.

*   Ignoring errors

# **Wrapping of error**

In go, error can wrap another error as well. 

What does wrapping of error mean? It means to create a hierarchy of errors in which a  particular instance of error wraps another error and that particular instance itself can be wrapped inside another error.  Below is the syntax for wrapping an error

```
e := fmt.Errorf("... %w ...", ..., err, ...)
```

**%w** directive Is used for wrapping the error.  The **fmt.Errorf **should be called with only one %w directive. Let’s see an example.

```
package main

import (
	"fmt"
)

type errorOne struct{}

func (e errorOne) Error() string {
	return "Error One happended"
}

func main() {

	e1 := errorOne{}

	e2 := fmt.Errorf("E2: %w", e1)

	e3 := fmt.Errorf("E3: %w", e2)

	fmt.Println(e2)

	fmt.Println(e3)

}
```

**Output**

```
E2: Error One happended
E3: E2: Error One happended
```

In the above program, we created a struct **errorOne** that has an **Error** method hence it implements the **error** interface. Then we created an instance of the **errorOne** struct named **e1**. Then we wrapped that instance **e1** into another error **e2** like this

```
e2 := fmt.Errorf("E2: %w", e1)
```

Then we wrapped **e2** into **e3** like below. 

```
e3 := fmt.Errorf("E3: %w", e2)
```

So so we created a hierarchy of errors in which **e3** wraps **e2** and **e2** wraps **e1**.  Thus **e3** also wraps **e1** transitively. When we print **e2 ** it also prints the error from **e1** and gives the output.

```
E2: Error One happended
```

When we print **e3** it prints the error from **e2** as well as **e1** and gives the output.

```
E3: E2: Error One happended
```

Now the question which comes to the mind that whats the use case of wrapping the errors. To understand it let’s see an example

```
package main

import (
	"fmt"
)

type notPositive struct {
	num int
}

func (e notPositive) Error() string {
	return fmt.Sprintf("checkPositive: Given number %d is not a positive number", e.num)
}

type notEven struct {
	num int
}

func (e notEven) Error() string {
	return fmt.Sprintf("checkEven: Given number %d is not an even number", e.num)
}

func checkPositive(num int) error {
	if num < 0 {
		return notPositive{num: num}
	}
	return nil
}

func checkEven(num int) error {
	if num%2 == 1 {
		return notEven{num: num}
	}
	return nil
}

func checkPostiveAndEven(num int) error {
	if num > 100 {
		return fmt.Errorf("checkPostiveAndEven: Number %d is greater than 100", num)
	}

	err := checkPositive(num)
	if err != nil {
		return err
	}

	err = checkEven(num)
	if err != nil {
		return err
	}

	return nil
}

func main() {
	num := 3
	err := checkPostiveAndEven(num)
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Println("Givennnumber is positive and even")
	}

}
```

**Output**

```
checkEven: Given number 3 is not an even number
```

In the above program, we have a function **checkPostiveAndEven** that checks whether a number is even and positive. In turns, it calls the **checkEven** function to check if the number is even. And then it calls **checkPositive** function to check if the number is positive. If a number is not even and positive it an error is raised.

In the above program it is impossible to tell stack trace of the error. We know that this error came from **checkEven** function for the above output. But which function called the **checkEven** function is not clear from the error. This is where wrapping the error comes in the picture.  This becomes more useful when the project is big and there are a lot of functions calling each other.  Let’s rewrite the program by wrapping the error.

```
package main

import (
	"fmt"
)

type notPositive struct {
	num int
}

func (e notPositive) Error() string {
	return fmt.Sprintf("checkPositive: Given number %d is not a positive number", e.num)
}

type notEven struct {
	num int
}

func (e notEven) Error() string {
	return fmt.Sprintf("checkEven: Given number %d is not an even number", e.num)
}

func checkPositive(num int) error {
	if num < 0 {
		return notPositive{num: num}
	}
	return nil
}

func checkEven(num int) error {
	if num%2 == 1 {
		return notEven{num: num}
	}
	return nil
}

func checkPostiveAndEven(num int) error {
	if num > 100 {
		return fmt.Errorf("checkPostiveAndEven: Number %d is greater than 100", num)
	}

	err := checkPositive(num)
	if err != nil {
		return fmt.Errorf("checkPostiveAndEven: %w", err)
	}

	err = checkEven(num)
	if err != nil {
		return fmt.Errorf("checkPostiveAndEven: %w", err)
	}

	return nil
}

func main() {
	num := 3
	err := checkPostiveAndEven(num)
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Println("Given number is positive and even")
	}

}
```

**Output**

```
checkPostiveAndEven: checkEven: Given number 3 is not an even number
```

 The above program is same as the previous program just that in the **checkPostiveAndEven** function , we wrap the errors like below.

```
fmt.Errorf("checkPostiveAndEven: %w", err)
```

So the output is more clear and the error is more informative. The output clearly mentions the sequence of calling as well

```
checkPostiveAndEven: checkEven: Given number 3 is not an even number
```

# **Unwrap an error**

In the above section, we studied about wrapping the error. It is also possible to unwrap the error. Unwrap function of errors package can be used to unwrap an error. Below is the syntax of the function.

```
func Unwrap(err error) error
```

If the **err** wraps another error, then the wrapped error will be returned otherwise **Unwrap** function will return nil.

Let’s see a program to illustrate the same

```
import (
    "errors"
    "fmt"
)
type errorOne struct{}
func (e errorOne) Error() string {
    return "Error One happened"
}
func main() {
    e1 := errorOne{}
    e2 := fmt.Errorf("E2: %w", e1)
    e3 := fmt.Errorf("E3: %w", e2)
    fmt.Println(errors.Unwrap(e3))
    fmt.Println(errors.Unwrap(e2))
    fmt.Println(errors.Unwrap(e1))
}
```

**Output**

```
E2: Error One happended
Error One happended 
```

In the above program, we created a struct **errorOne** that has an **Error** method hence it implements the **error** interface. Then we created an instance of the **errorOne** struct named **e1**. Then we wrapped that instance **e1** into another error **e2** like this

```
e2 := fmt.Errorf("E2: %w", e1)
```

Then we wrapped **e2** into **e3** like below. 

```
e3 := fmt.Errorf("E3: %w", e2)
```

Hence

```
fmt.Println(errors.Unwrap(e3))
```

will return wrapped error **e2** as **e3** wraps **e2** and output will be

```
E2: Error One happened
```

While

```
fmt.Println(errors.Unwrap(e1))
```

will output nil as **e1** does not wraps any error

```
{nil}
```

# **Check if two error are equal**

First of all, what is meant by equality of the error?  As you already know that error is represented by the error interface in go. In go, two interfaces are equal if

*   Both refer to the same underlying type

*   The underlying value is equal (or both nil)

So above two points apply for comparing the error as well. There are two ways of checking if given errors are equal

## Using the equality operator (==)

**==** operator ca be used to compare two error in golang

## Using the **Is** function of errors package

[https://golang.org/pkg/errors/](https://golang.org/pkg/errors/) .  Using **Is** function is preferable to using the equality operator because it checks for equality by unwrapping the first error sequentially and matches it with target error at each step of unwrap. We will see an example later on to fully understand why it is preferable. Below is the syntax of Is function.

```
func Is(err, target error) bool
```

 Let’s see an example

```
package main
import (
    "errors"
    "fmt"
)
type errorOne struct{}
func (e errorOne) Error() string {
    return "Error One happended"
}
func main() {
    var err1 errorOne
    err2 := do()
    if err1 == err2 {
        fmt.Println("Equality Operator: Both errors are equal")
    }
    if errors.Is(err1, err2) {
        fmt.Println("Is function: Both errors are equal")
    }
}
func do() error {
    return errorOne{}
}
```

**Output**

```
Equality Operator: Both errors are equal
Is function: Both errors are equal
```

In the above program, we created **errorOne** struct which defines the **Error** method hence implement the **error** interface.  We created **err1**  variable which is an instance of **errorOne** struct. We also created a **do()** function which raises an error of type **errorOne** and that is captured in **err2** variable in the main function

Then we are comparing two errors using

*   Using the equality operator

```
err1 == err2
```

*   Using the **Is** function of errors package

```
errors.Is(err1, err2)
```

Both the method correctly output that the error is equal as both **err1** and **err2**

*   Refer to same underlying type which is **errorOne**

*   Have the same underlying value

We mentioned above that using the **Is** function preferable to using the equality operator because it checks for equality by unwrapping the first error sequentially and matches it with the target error at each step of unwrap. Let’s see an example of that

```
package main

import (
	"errors"
	"fmt"
)

type errorOne struct{}

func (e errorOne) Error() string {
	return "Error One happended"
}

func main() {
	err1 := errorOne{}

	err2 := do()

	if err1 == err2 {
		fmt.Println("Equality Operator: Both errors are equal")
	} else {
		fmt.Println("Equality Operator: Both errors are not equal")
	}

	if errors.Is(err2, err1) {
		fmt.Println("Is function: Both errors are equal")
	}
}

func do() error {
	return fmt.Errorf("E2: %w", errorOne{})
}
```

**Output**

```
Equality Operator: Both errors are not equal
Is function: Both errors are equal
```

The above program is almost same as the previous program just the difference being that in the **do()** function we are wrapping the error as well

```
return fmt.Errorf("E2: %w", errorOne{})
```

*   The equality operator outputs

```
Equality Operator: Both errors are not equal
```

*   While **Is** function outputs

```
Is function: Both errors are equal
```

This is because the **err2** returned wraps an instance of  **errorOne** which is not catched by the equality operator but is catched by **Is** function

# **Get the underlying error from an error represented by the error interfac**e

There are two ways of getting the underlying type

## Using the .({type}) assert

If the assert succeeds then it will return the corresponding error otherwise it will panic. Below is the syntax

```
err := err.({type})
```

Better to use **ok** variable to prevent panic in case assertion fails.  Below is the syntax for that. The **ok** variable will be set to true if  error underlying type is correct

```
err, ok := err.({type})
```

## **Using the As function of errors package**

[https://golang.org/pkg/errors/](https://golang.org/pkg/errors/) . Using **As** function is preferable to using the .({type}) assert because it checks for match by unwrapping the first error sequentially and matches it with target error at each step of unwrap. Below is the syntax of Is function

```
func As(err error, target interface{}) bool
```

**As** function will find the first error in the first argument which can match the target. Once a match is found it will set target to that error value.

Let’s see an example

```
package main

import (
	"errors"
	"fmt"
	"os"
)

func main() {

	err := openFile("non-existing.txt")

	if e, ok := err.(*os.PathError); ok {
		fmt.Printf("Using Assert: Error e is of type path error. Path: %v\n", e.Path)
	} else {
		fmt.Println("Using Assert: Error not of type path error")
	}

	var pathError *os.PathError
	if errors.As(err, &pathError) {
		fmt.Printf("Using As function: Error e is of type path error. Path: %v\n", pathError.Path)
	}
}

func openFile(fileName string) error {
	_, err := os.Open("non-existing.txt")
	if err != nil {
		return err
	}
	return nil
}
```

**Output:**

```
Using Assert: Error e is of type path error. Path: non-existing.txt
Using As function: Error e is of type path error. Path: non-existing.txt
```

In the above program, we have a function openFile in which we are trying to open a non-existing type hence it will raise an error. Then we are asserting  the error in two ways

*   Using . assert operator. The **ok** variable will be set to true if  error underlying type is ***os.PathError** otherwise it will be set to false

```
e,ok := err.(*os.PathError); ok
```

*   Using the **As** function of errors package

```
errors.As(err, &pathError)
```

Both the method correctly assert that the error is of type ***os.PathError** as error returned by the **openFile** function is of type ***os.PathError**

We mentioned above that using **As** function is preferable to using the .({type}) assert because it checks for a match by unwrapping the first error sequentially and matches it with the target error at each step of unwrap. Let’s see an example to understand that

```
import (
	"errors"
	"fmt"
	"os"
)

func main() {
	var pathError *os.PathError
	err := openFile("non-existing.txt")

	if e, ok := err.(*os.PathError); ok {
		fmt.Printf("Using Assert: Error e is of type path error. Error: %v\n", e)
	} else {
		fmt.Println("Using Assert: Error not of type path error")
	}

	if errors.As(err, &pathError) {
		fmt.Printf("Using As function: Error e is of type path error. Error: %v\n", pathError)
	}
}

func openFile(fileName string) error {
	_, err := os.Open("non-existing.txt")
	if err != nil {
		return fmt.Errorf("Error opening: %w", err)
	}
	return nil
}
```

**Output:**

```
Using Assert: Error not of type path error
Using As function: Error e is of type path error. Error: open non-existing.txt: no such file or directory
```

The above program is almost same as the previous program just the difference being that in the **openFile** function we are wrapping the error as well

```
return fmt.Errorf("Error opening: %w", err)
```

*   The . assert outputs

```
Using Assert: Error not of type path error
```

*   While As function outputs

```
Using As function: Error e is of type path error. Error: open non-existing.txt: no such file or directory
```

This is because the error returned by the **openFile** function wraps ***os.Patherror** error which is not catched by the dot(‘.’) assert but is catched by **As** function

# **Conclusion**

This is all about advanced topic in error in golang. Hope you have liked this article. Please share feedback/improvements/mistakes in comments.

**Next Tutorial** – [Panic and Recover](https://golangbyexample.com/panic-and-recover-golang/)
**Previous Tutorial** – [Error – Part 1](https://golangbyexample.com/error-in-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*