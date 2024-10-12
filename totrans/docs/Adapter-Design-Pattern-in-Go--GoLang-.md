<!--yml
category: 未分类
date: 2024-10-13 06:02:10
-->

# Adapter Design Pattern in Go (GoLang)

> 来源：[https://golangbyexample.com/adapter-design-pattern-go/](https://golangbyexample.com/adapter-design-pattern-go/)

Note: Interested in understanding how all other design patterns can be implemented in GO. Please see this full reference – [All Design Patterns in Go (Golang)](https://golangbyexample.com/all-design-patterns-golang/)

Table of Contents

 **   [Introduction:](#Introduction "Introduction: ")
*   [When to Use](#When_to_Use "When to Use")
*   [UML Diagram](#UML_Diagram "UML Diagram")
*   [Mapping ](#Mapping "Mapping ")
*   [Example:](#Example "Example:")*  *# **Introduction:**

This design pattern is a Structural Design Pattern. The patter is best understood with an example. Let’s say you have two laptops

1.  MacBook Pro
2.  Windows Laptop

MacBook Pro has a USB port that is **square** in shape and Windows have a USB port that is **circular** in shape. You as a client have a USB cable that is square in shape so it can only be inserted in the mac laptop. So you see the problem here

**Problem:**

*   We have a class (Client) that is expecting some features of an object (square USB port here), but we have another object called adaptee (Windows Laptop here) which offers the same functionality but through a different interface( circular port)

This is where Adapter Pattern comes into the picture. We create a class known as Adapter that will

*   Adhere to the same interface which client expects ( Square USB port here)
*   Translate the request from the client to the adaptee in the form that adaptee expects. Basically, in our example act as an adapter that accepts USB in square port and then inserts into circular port in windows laptop.

# **When to Use**

*   Use this design pattern when the objects implement a different interface as required by the client.

# **UML Diagram**

![](img/6632e3859f160ee0d25f02db10d4c996.png)

Below is the corresponding mapping UML diagram with the example given above

![](img/d950f0f1c8f9c52eefd01e3dcb144658.png)

# **Mapping **

The below table represents the mapping from the UML diagram actors to actual implementation actors in code.

| Target | computer.go |
| Concrete Prototype 1 | mac.go |
| Concrete Prototype 2 (Adapter) | windowsAdapter.go |
| adaptee | windows.go |
| client | client.go |

# **Example**:

**computer.go**

```
package main

type computer interface {
    insertInSquarePort()
}
```

**mac.go**

```
package main

import "fmt"

type mac struct {
}

func (m *mac) insertInSquarePort() {
    fmt.Println("Insert square port into mac machine")
}
```

**windowsAdapter.go**

```
package main

type windowsAdapter struct {
	windowMachine *windows
}

func (w *windowsAdapter) insertInSquarePort() {
	w.windowMachine.insertInCirclePort()
} 
```

**windows.go**

```
package main

import "fmt"

type windows struct{}

func (w *windows) insertInCirclePort() {
    fmt.Println("Insert circle port into windows machine")
}
```

**client.go**

```
package main

type client struct {
}

func (c *client) insertSquareUsbInComputer(com computer) {
    com.insertInSquarePort()
}
```

**main.go**

```
package main

func main() {
    client := &client{}
    mac := &mac{}
    client.insertSquareUsbInComputer(mac)
    windowsMachine := &windows{}
    windowsMachineAdapter := &windowsAdapter{
        windowMachine: windowsMachine,
    }
    client.insertSquareUsbInComputer(windowsMachineAdapter)
}
```

**Output:**

```
Insert square port into mac machine
Insert circle port into windows machine
```

*   [adapter](https://golangbyexample.com/tag/adapter/)*   [adapter design pattern](https://golangbyexample.com/tag/adapter-design-pattern/)*   [design](https://golangbyexample.com/tag/design/)*   [design pattern](https://golangbyexample.com/tag/design-pattern/)*