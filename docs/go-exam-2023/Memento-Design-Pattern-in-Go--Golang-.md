<!--yml

类别：未分类

日期：2024-10-13 06:04:05

-->

# Go 语言中的备忘录设计模式（Memento Design Pattern in Go (Golang)）

> 来源：[`golangbyexample.com/memento-design-pattern-go/`](https://golangbyexample.com/memento-design-pattern-go/)

注意：如有兴趣了解其他设计模式在 Go 中的实现，请查看此完整参考 – [Go 语言中的所有设计模式](https://golangbyexample.com/all-design-patterns-golang/)

目录

**介绍：

+   实际例子：

+   完整工作代码：*  *# **介绍：**

备忘录设计模式是一种行为设计模式。它允许我们为对象保存检查点，从而使对象能够恢复到之前的状态。基本上，它有助于对象的撤销-重做操作。以下是备忘录设计模式的设计组件。

+   **发起者**：这是实际保存状态的对象，状态被保存为备忘录。

+   **备忘录**：这是保存发起者状态的对象。

+   **照管者**：这是保存多个备忘录的对象。给定一个索引，它返回相应的备忘录。

发起者定义了两个方法。**savememento()**和**restorememento()**

+   **savememento()-** 在此方法中，发起者将其内部状态保存到备忘录对象中。

+   **restorememento()-** 此方法以备忘录对象为输入。发起者将自身恢复到过去的备忘录。因此，恢复了之前的状态。

# **实际例子：**

**originator.go**

```go
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

```go
package main

type memento struct {
    state string
}

func (m *memento) getSavedState() string {
    return m.state
}
```

**caretaker.go**

注意，照管者包含了 mementoArray，它保存了所有备忘录。

```go
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

```go
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

**输出：**

```go
originator Current State: A
originator Current State: B
originator Current State: C
Restored to State: B
Restored to State: A 
```

# **完整工作代码：**

```go
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

**输出：**

```go
originator Current State: A
originator Current State: B
originator Current State: C
Restored to State: B
Restored to State: A
```

+   [golang](https://golangbyexample.com/tag/golang/)*   [memento](https://golangbyexample.com/tag/memento/)*   [pattern](https://golangbyexample.com/tag/pattern/)*
