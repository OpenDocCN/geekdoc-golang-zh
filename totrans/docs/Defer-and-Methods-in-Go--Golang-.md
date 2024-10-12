<!--yml
category: 未分类
date: 2024-10-13 06:27:23
-->

# Defer and Methods in Go (Golang)

> 来源：[https://golangbyexample.com/defer-methods-golang/](https://golangbyexample.com/defer-methods-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

**defer** statement is also applicable  for methods in a similar way it is applicable to functions. Let’s see an example

# **Example**

```
package main

import (
    "fmt"
    "log"
    "os"
)

func main() {
    err := writeToTempFile("Some text")
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Printf("Write to file succesful")
}

func writeToTempFile(text string) error {
    file, err := os.Open("temp.txt")
    if err != nil {
        return err
    }
    defer file.Close()

    n, err := file.WriteString("Some text")
    if err != nil {
        return err
    }
    fmt.Printf("Number of bytes written: %d", n)
    return nil
}
```

In the above program, we do **defer file.Close()** after opening the file. **Close** is the method which is defined on the **file** instance. This will make sure that closing of the file is executed even if the write to the file results into an error. Defer function makes sure that the file will be closed regardless of number of return statements in the function.

Let’s see another example of a method called on a custom struct in a defer function

```
package main

import (
	"errors"
	"fmt"
)

type employee struct {
	name string
}

func (e *employee) setName(name string) error {
	defer e.setDefaultName()
	if len(name) < 3 {
		fmt.Println("Length of name passed is less than 3")
		return errors.New("Length of name cannnot be less than 3")
	}
	e.name = name
	return nil
}

func (e *employee) setDefaultName() {
        fmt.Println("In the setDefaultName function")
	if e.name == "" {
		e.name = "DefaultName"
		fmt.Println("Default name is set")
	}
}

func main() {
	e1 := &employee{}
	e1.setName("John")
	fmt.Printf("First employee name is: %s\n", e1.name)

	fmt.Println()
	e2 := &employee{}
	e2.setName("Ko")
	fmt.Printf("Second employee name is: %s\n", e2.name)
	return
}
```

**Output**

```
In the setDefaultName function
First employee name is: John

Length of name passed is less than 3
In the setDefaultName function
Default name is set
Second employee name is: DefaultName
```

In the above program we have a custom struct **employee**

```
type employee struct {
	name string
}
```

struct employee has a **setName** function which sets the name. But this function also raises an error is length of name passed is less than 3.  We have a defer function in the **setName** which executes after **setName** finishes and checks weather the name is empty. If it is empty then it sets a default name. This defer function is actually a method **setDefaultName**

```
func (e *employee) setDefaultName() {
	if e.name == "" {
		e.name = "DefaultName"
		fmt.Println("Default name is set")
	}
}
```

Then we create an instance of employee e1 and sets its name

Since length of "John" is greater than 3, the **setName** function doesn't raises an error. But if you will notice the output,  the defer function **setDefaultName** is still executed

```
In the setDefaultName function
First employee name is: John
```

Then we create an instance of employee **e2** and sets its name

```
e1 := &employee{}
e1.setName("Ko")
```

Since length of "Ko" is less than 3, the **setName** function raises  an error. defer function **setDefaultName** also gets executed in this case and it sets the default name. That is why you see below output in this case

```
Length of name passed is less than 3
In the setDefaultName function
Default name is set
Second employee name is: DefaultNam
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*