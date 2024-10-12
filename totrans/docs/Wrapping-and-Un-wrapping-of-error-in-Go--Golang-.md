<!--yml
category: 未分类
date: 2024-10-13 06:25:07
-->

# Wrapping and Un-wrapping of error in Go (Golang)

> 来源：[https://golangbyexample.com/wrapping-and-unwrapping-error-golang/](https://golangbyexample.com/wrapping-and-unwrapping-error-golang/)

Table of Contents

 **   [Wrapping of error](#Wrapping_of_error "Wrapping of error")
*   [Unwrap an error](#Unwrap_an_error "Unwrap an error")*  *# **Wrapping of error**

In go, error can wrap another error as well.  What does the wrapping of error mean? It means to create a hierarchy of errors in which a  particular instance of error wraps another error and that particular instance itself can be wrapped inside another error.  Below is the syntax for wrapping an error

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

In the above program, we have a function **checkPostiveAndEven** that checks whether a number is even and positive. In turn, it calls the **checkEven** function to check if the number is even. And then it calls **checkPositive** function to check if the number is positive. If a number is not even and positive it an error is raised.

In the above program, it is impossible to tell stack trace of the error. We know that this error came from **checkEven** function for the above output. But which function called the **checkEven** function is not clear from the error. This is where wrapping the error comes in the picture.  This becomes more useful when the project is big and there are a lot of functions calling each other.  Let’s rewrite the program by wrapping the error.

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

In the above section, we studied about wrapping the error. It is also possible to unwrap the error. **Unwrap** function of **errors** package can be used to unwrap an error. Below is the syntax of the function.

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
    return "Error One happended"
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

will return wrapped error **e2,** as **e3** wraps **e2** and output will be

```
E2: Error One happended
```

Also,

```
fmt.Println(errors.Unwrap(e2))
```

will return wrapped error **e1** as **e2** further wraps **e1** and output will be

```
Error One happened
```

While

```
fmt.Println(errors.Unwrap(e1))
```

will output nil as **e1** does not wraps any error

```
{nil}
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*