<!--yml

分类：未分类

日期：2024-10-13 06:27:23

-->

# Go语言中的延迟和方法

> 来源：[https://golangbyexample.com/defer-methods-golang/](https://golangbyexample.com/defer-methods-golang/)

目录

**   [概述](#Overview "Overview")

+   [示例](#Example "Example")*  *# **概述**

**defer**语句同样适用于方法，就像它适用于函数一样。让我们看看一个示例。

# **示例**

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

在上述程序中，我们在打开文件后做**defer file.Close()**。**Close**是定义在**file**实例上的方法。这确保了即使写入文件出现错误，文件也会被关闭。延迟函数确保无论函数中有多少个返回语句，文件都会被关闭。

让我们看看一个在延迟函数中调用自定义结构体的方法的另一个示例。

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

**输出**

```
In the setDefaultName function
First employee name is: John

Length of name passed is less than 3
In the setDefaultName function
Default name is set
Second employee name is: DefaultName
```

在上述程序中，我们有一个自定义结构体**employee**。

```
type employee struct {
	name string
}
```

结构体**employee**有一个**setName**函数，用于设置名称。但如果传入的名称长度小于3，该函数也会抛出错误。我们在**setName**中有一个延迟函数，它在**setName**完成后执行，并检查名称是否为空。如果为空，它会设置一个默认名称。这个延迟函数实际上是一个方法**setDefaultName**。

```
func (e *employee) setDefaultName() {
	if e.name == "" {
		e.name = "DefaultName"
		fmt.Println("Default name is set")
	}
}
```

然后我们创建一个**employee**实例e1并设置它的名称。

由于“John”的长度大于3，**setName**函数不会抛出错误。但如果你注意输出，延迟函数**setDefaultName**仍然被执行。

```
In the setDefaultName function
First employee name is: John
```

然后我们创建一个**employee**实例**e2**并设置它的名称。

```
e1 := &employee{}
e1.setName("Ko")
```

由于“Ko”的长度小于3，**setName**函数抛出错误。在这种情况下，延迟函数**setDefaultName**也会执行，并设置默认名称。这就是为什么你在下面的输出中看到这种情况。

```
Length of name passed is less than 3
In the setDefaultName function
Default name is set
Second employee name is: DefaultNam
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
