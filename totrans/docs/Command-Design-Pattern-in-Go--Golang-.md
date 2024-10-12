<!--yml
category: 未分类
date: 2024-10-13 06:03:04
-->

# Command Design Pattern in Go (Golang)

> 来源：[https://golangbyexample.com/command-design-pattern-in-golang/](https://golangbyexample.com/command-design-pattern-in-golang/)

Note: Interested in understanding how all other design patterns can be implemented in GO. Please see this full reference – [https://golangbyexample.com/all-design-patterns-golang/](https://golangbyexample.com/all-design-patterns-golang/https://golangbyexample.com/all-design-patterns-golan)

Table of Contents

 **   [Introduction:](#Introduction "Introduction:")
*   [UML Diagram:](#UML_Diagram "UML Diagram:")
*   [Mapping ](#Mapping "Mapping ")
*   [Practical Example:](#Practical_Example "Practical Example:")
*   [Full Working Code:](#Full_Working_Code "Full Working Code:")*  *## **Introduction:**

Command Design Pattern is a behavioral design pattern. It suggests encapsulating the request as a standalone object. The created object has all the information about the request and thus can execute it independently.

The basic components that are used in the command design pattern are:

*   **Receiver** – It is the class which contains the business logic. The command object only delays its requests to the receiver.
*   **Command** – embeds receiver and binds a particular action of the receiver.
*   **Invoker** – It embeds the command and envokes the command by calling the command’s execute method.
*   **Client** – It creates the command with the appropriate receiver bypassing the receiver to the command’s constructor. After that, it also associates the resulting command with an invoker.

Let’s understand a situation after which it will be clear why the command pattern is useful.  Imagine the case of a TV. A TV can be turned **ON** by either

1.  Remote ON Button
2.  On Button on the tv.

Both these trigger points do the same thing i.e. turn the TV on. In order to ON the TV, we can implement the ON command object with the receiver as the TV. When execute() method is called on this ON command object, it in turn call TV.on() function. So in this case:

*   **Receiver** is the **TV**
*   **Command** is the **ON command object** which embeds **TV**
*   **Invoker** is the **Remote ON Button** or the **ON Button on the TV**. Both embed the **ON command object**

Notice here that we have wrapped the request of turning the **TV** on into an **ON command object** which can be invoked by multiple invokers. This ON command object embeds the receiver (TV here) and can be executed independently.

As another example, imagine the case of an **Adobe Photoshop** Application. In Photoshop a Save operation can be triggered from 3 places

1.  From the menu.
2.  From the button on the upper bar.
3.  Using shortcut Ctrl+S.

All three trigger points do the same thing, i.e save the current image in the application. This saves can be wrapped into a Save Command Object with a current image open in the application as the receiver.

What’s the benefit of creating a separate command object in the above examples.

1.  It decouples the UI logic from underlying business logic
2.  No need to create different handlers for each of the invokers.
3.  The command object contains all the information it needs to execute. Hence it can also be used for delayed execution.

Let’s look at the UML diagram now.

## **UML Diagram:**

*   Notice how Invoker embeds the command. The request is sent to the Invoker and it passes the request to the encapsulated command object.
*   All the Concrete Command Object embed the receiver

*   ![](img/5cdfb3ea5569210aaa3c225e0fba2611.png)

## **Mapping **

The below table represents the mapping from the UML diagram actors to actual implementation actors in **“Practical Example”** below

| Invoker | button.go |
| Command Interface | command.go |
| Concrete Command 1 | onCommand.go |
| Concrete Command 2 | offCommand.go |
| Receiver Interface | device.go |
| Concrete Receiver | tv.go |
| Client | main.go |

## **Practical Example:**

**button.go**

```
package main

type button struct {
    command command
}

func (b *button) press() {
    b.command.execute()
}
```

**command.go**

```
package main

type command interface {
    execute()
}
```

**onCommand.go**

```
package main

type onCommand struct {
    device device
}

func (c *onCommand) execute() {
    c.device.on()
}
```

**offCommand.go**

```
package main

type offCommand struct {
    device device
}

func (c *offCommand) execute() {
    c.device.off()
}
```

**device.go**

```
package main

type device interface {
    on()
    off()
}
```

**tv.go**

```
package main

import "fmt"

type tv struct {
    isRunning bool
}

func (t *tv) on() {
    t.isRunning = true
    fmt.Println("Turning tv on")
}

func (t *tv) off() {
    t.isRunning = false
    fmt.Println("Turning tv off")
}
```

**main.go**

```
package main

func main() {
    tv := &tv{}
    onCommand := &onCommand{
        device: tv,
    }
    offCommand := &offCommand{
        device: tv,
    }
    onButton := &button{
        command: onCommand,
    }
    onButton.press()
    offButton := &button{
        command: offCommand,
    }
    offButton.press()
}
```

**Output:**

```
Turning tv on
Turning tv off
```

## **Full Working Code:**

```
package main

import "fmt"

type button struct {
    command command
}

func (b *button) press() {
    b.command.execute()
}

type command interface {
    execute()
}

type offCommand struct {
    device device
}

func (c *offCommand) execute() {
    c.device.off()
}

type onCommand struct {
    device device
}

func (c *onCommand) execute() {
    c.device.on()
}

type device interface {
    on()
    off()
}

type tv struct {
    isRunning bool
}

func (t *tv) on() {
    t.isRunning = true
    fmt.Println("Turning tv on")
}

func (t *tv) off() {
    t.isRunning = false
    fmt.Println("Turning tv off")
}

func main() {
    tv := &tv{}
    onCommand := &onCommand{
        device: tv,
    }
    offCommand := &offCommand{
        device: tv,
    }
    onButton := &button{
        command: onCommand,
    }
    onButton.press()
    offButton := &button{
        command: offCommand,
    }
    offButton.press()
}
```

**Output:**

```
Turning tv on
Turning tv off
```

*   [command design pattern in go](https://golangbyexample.com/tag/command-design-pattern-in-go/)*   [design pattern](https://golangbyexample.com/tag/design-pattern/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*