<!--yml
category: 未分类
date: 2024-10-13 06:24:54
-->

# Comparing error or error equality in Go (Golang)

> 来源：[https://golangbyexample.com/comparing-error-go/](https://golangbyexample.com/comparing-error-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

First of all, what is meant by equality of the error?  As you already know that error is represented by the error interface in go. In go, two interfaces are equal if

*   Both refer to the same underlying type

*   The underlying value is equal (or both nil)

So the above two points apply for comparing the error as well. There are two ways of checking if given errors are equal

*   Using the equality operator (==)

*   Using the Is function of errors package – [https://golang.org/pkg/errors/](https://golang.org/pkg/errors/) .  Using Is function is preferable to using the equality operator because it checks for equality by unwrapping the first error sequentially and matches it with the target error at each step of unwrap. We will see an example, later on, to fully understand why it is preferable. Below is the syntax of Is function.

```
func Is(err, target error) bool
```

# **Code**

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

*   Using equality operator

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

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*