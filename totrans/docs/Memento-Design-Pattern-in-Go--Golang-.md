<!--yml
category: 未分类
date: 2024-10-13 06:04:05
-->

# Memento Design Pattern in Go (Golang)

> 来源：[https://golangbyexample.com/memento-design-pattern-go/](https://golangbyexample.com/memento-design-pattern-go/)

Note: Interested in understanding how all other design patterns can be implemented in GO. Please see this full reference – [All Design Patterns in Go (Golang)](https://golangbyexample.com/all-design-patterns-golang/)

Table of Contents

 **   [Introduction:](#Introduction "Introduction:")
*   [Practical Example:](#Practical_Example "Practical Example:")
*   [Full Working Code:](#Full_Working_Code "Full Working Code:")*  *# **Introduction:**

Memento design pattern is a behavioral design pattern. It allows us to save checkpoints for an object and thus allow an object to revert to its previous state. Basically it helps in undo-redo operations on an object. Below are the design components of the Memento Design Pattern.

*   **Originator**: It is the actual object whose state is saved as a memento. 
*   **Memento**: This is the object which saves the state of the originator
*   **Caretaker**: This is the object that saves multiple mementos. Given an index, it returns the corresponding memento. 

The originator defines two methods. **savememento()** and **restorememento()**

*   **savememento()-** in this method the originator saves its internal state into a memento object.
*   **restorememento()-** this method takes input as a memento object. The originator restores itself to the pass memento.  Hence a previous state is restored.

# **Practical Example:**

**originator.go**

```
package main

type originator struct {
    state string
}

func (e *originator) createMemento() *memento {
    return &memento{state: e.state}
}

func (e *originator) restorememento(m *memento) {
    e.state = m.getSavedState()
}

func (e *originator) setState(state string) {
    e.state = state
}

func (e *originator) getState() string {
    return e.state
```

**memento.go**

```
package main

type memento struct {
    state string
}

func (m *memento) getSavedState() string {
    return m.state
}
```

**caretaker.go**

Notice that the caretaker contains the mementoArray which holds all the memento.

```
package main

type caretaker struct {
    mementoArray []*memento
}

func (c *caretaker) addMemento(m *memento) {
    c.mementoArray = append(c.mementoArray, m)
}

func (c *caretaker) getMenento(index int) *memento {
    return c.mementoArray[index]
}
```

**main.go**

```
package main

import "fmt"

func main() {
    caretaker := &caretaker{
        mementoArray: make([]*memento, 0),
    }
    originator := &originator{
        state: "A",
    }
    fmt.Printf("Originator Current State: %s\n", originator.getState())
    caretaker.addMemento(originator.createMemento())

    originator.setState("B")
    fmt.Printf("Originator Current State: %s\n", originator.getState())

    caretaker.addMemento(originator.createMemento())
    originator.setState("C")

    fmt.Printf("Originator Current State: %s\n", originator.getState())
    caretaker.addMemento(originator.createMemento())

    originator.restorememento(caretaker.getMenento(1))
    fmt.Printf("Restored to State: %s\n", originator.getState())

    originator.restorememento(caretaker.getMenento(0))
    fmt.Printf("Restored to State: %s\n", originator.getState())
}
```

**Output:**

```
originator Current State: A
originator Current State: B
originator Current State: C
Restored to State: B
Restored to State: A 
```

# **Full Working Code:**

```
package main

import "fmt"

type originator struct {
    state string
}

func (e *originator) createMemento() *memento {
    return &memento{state: e.state}
}

func (e *originator) restoreState(m *memento) {
    e.state = m.getSavedState()
}

func (e *originator) setState(state string) {
    e.state = state
}

func (e *originator) getState() string {
    return e.state
}

type memento struct {
    state string
}

func (m *memento) getSavedState() string {
    return m.state
}

type caretaker struct {
    mementoArray []*memento
}

func (c *caretaker) addMemento(m *memento) {
    c.mementoArray = append(c.mementoArray, m)
}

func (c *caretaker) getMenento(index int) *memento {
    return c.mementoArray[index]
}

func main() {
    caretaker := &caretaker{
        mementoArray: make([]*memento, 0),
    }
    originator := &originator{
        state: "A",
    }
    fmt.Printf("Originator Current State: %s\n", originator.getState())
    caretaker.addMemento(originator.createMemento())
    originator.setState("B")
    fmt.Printf("Originator Current State: %s\n", originator.getState())
    caretaker.addMemento(originator.createMemento())
    originator.setState("C")
    fmt.Printf("Originator Current State: %s\n", originator.getState())
    caretaker.addMemento(originator.createMemento())
    originator.restoreState(caretaker.getMenento(1))
    fmt.Printf("Restored to State: %s\n", originator.getState())
    originator.restoreState(caretaker.getMenento(0))
    fmt.Printf("Restored to State: %s\n", originator.getState())
}
```

**Output:**

```
originator Current State: A
originator Current State: B
originator Current State: C
Restored to State: B
Restored to State: A
```

*   [golang](https://golangbyexample.com/tag/golang/)*   [memento](https://golangbyexample.com/tag/memento/)*   [pattern](https://golangbyexample.com/tag/pattern/)*