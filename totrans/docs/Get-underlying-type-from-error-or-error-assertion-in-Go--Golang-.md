<!--yml
category: 未分类
date: 2024-10-13 06:25:00
-->

# Get underlying type from error or error assertion in Go (Golang)

> 来源：[https://golangbyexample.com/error-assertion-golang/](https://golangbyexample.com/error-assertion-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

There are two ways of getting the underlying type

*   Using the .({type}) assert

If the assert succeeds then it will return the corresponding error otherwise it will panic. Below is the syntax

```
err := err.({type})
```

Better to use **ok** variable to prevent panic in case assertion fails.  Below is the syntax for that. The **ok** variable will be set to true if  error underlying type is correct

```
err, ok := err.({type})
```

Using the **As** function of **errors** package-  [https://golang.org/pkg/errors/](https://golang.org/pkg/errors/) .Using **As** function is preferable to using the .({type}) assert because it checks for match by unwrapping the first error sequentially and matches it with target error at each step of unwrap. Below is the syntax of Is function

```
func As(err error, target interface{}) bool
```

**As** function will find the first error in the first argument which can match the target. Once a match is found it will set target to that error value.

# **Code**

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

*   Using .assert operator. The ok variable will be set to true if the error underlying type is ***os.PathError** otherwise it will be set to false

```
e,ok := err.(*os.PathError); ok
```

*   Using the **As** function of errors package

```
errors.As(err, &pathError)
```

Both the method correctly assert that the error is of type ***os.PathError** as error returned by the **openFile** function is of type ***os.PathError**

We mentioned above that using **As** the function is preferable to using the .({type}) assert because it checks for a match by unwrapping the first error sequentially and matches it with the target error at each step of unwrap. Let’s see an example to understand that

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

The above program is almost same as the previous program just the difference being that in the openFile function we are wrapping the error as well

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

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*