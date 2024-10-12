<!--yml
category: 未分类
date: 2024-10-13 06:04:10
-->

# Object Pool Design Pattern in Go (Golang)

> 来源：[https://golangbyexample.com/golang-object-pool/](https://golangbyexample.com/golang-object-pool/)

Note: Interested in understanding how all other design patterns can be implemented in GO. Please see this full reference – [All Design Patterns in Go (Golang)](https://golangbyexample.com/all-design-patterns-golang/)

Table of Contents

 **   [Introduction: ](#Introduction "Introduction:  ")
*   [When to Use:](#When_to_Use "When to Use:")
*   [Example:](#Example "Example:")
*   [Full Working Code:](#Full_Working_Code "Full Working Code:")*  *## **Introduction: **

The Object Pool Design Pattern is a creational design pattern in which a pool of objects is initialized and created beforehand and kept in a pool. As and when needed, a client can request an object from the pool, use it, and return it to the pool. The object in the pool is never destroyed.

## **When to Use:**

*   When the cost to create the object of the class is high and the number of such objects that will be needed at a particular time is not much.

-Let’s take the example of DB connections. Each of the connection object creation is cost is high as there is network calls involved and also at a time not more than a certain connection might be needed. The object pool design pattern is perfectly suitable for such cases.

*   When the pool object is the immutable object

         -Again take the example of DB connection again. A DB connection is an immutable object. Almost none of its property needs to be changed

*   For performance reasons. It will boost the application performance significantly since the pool is already created

## **Example:**

**iPoolObject.go**

```
package main

type iPoolObject interface {
    getID() string //This is any id which can be used to compare two different pool objects
}
```

**pool.go**

```
package main

import (
    "fmt"
    "sync"
)

type pool struct {
    idle   []iPoolObject
    active []iPoolObject
    capacity int
    mulock   *sync.Mutex
}

//InitPool Initialize the pool
func initPool(poolObjects []iPoolObject) (*pool, error) {
    if len(poolObjects) == 0 {
        return nil, fmt.Errorf("Cannot craete a pool of 0 length")
    }
    active := make([]iPoolObject, 0)
    pool := &pool{
        idle:     poolObjects,
        active:   active,
        capacity: len(poolObjects),
        mulock:   new(sync.Mutex),
    }
    return pool, nil
}

func (p *pool) loan() (iPoolObject, error) {
    p.mulock.Lock()
    defer p.mulock.Unlock()
    if len(p.idle) == 0 {
        return nil, fmt.Errorf("No pool object free. Please request after sometime")
    }
    obj := p.idle[0]
    p.idle = p.idle[1:]
    p.active = append(p.active, obj)
    fmt.Printf("Loan Pool Object with ID: %s\n", obj.getID())
    return obj, nil
}

func (p *pool) receive(target iPoolObject) error {
    p.mulock.Lock()
    defer p.mulock.Unlock()
    err := p.remove(target)
    if err != nil {
        return err
    }
    p.idle = append(p.idle, target)
    fmt.Printf("Return Pool Object with ID: %s\n", target.getID())
    return nil
}

func (p *pool) remove(target iPoolObject) error {
    currentActiveLength := len(p.active)
    for i, obj := range p.active {
        if obj.getID() == target.getID() {
            p.active[currentActiveLength-1], p.active[i] = p.active[i], p.active[currentActiveLength-1]
            p.active = p.active[:currentActiveLength-1]
            return nil
        }
    }
    return fmt.Errorf("Targe pool object doesn't belong to the pool")
}
```

**connection.go**

```
package main

type connection struct {
    id string
}

func (c *connection) getID() string {
    return c.id
}
```

**main.go**

```
package main

import (
    "log"
    "strconv"
)

func main() {
    connections := make([]iPoolObject, 0)
    for i := 0; i < 3; i++ {
        c := &connection{id: strconv.Itoa(i)}
        connections = append(connections, c)
    }
    pool, err := initPool(connections)
    if err != nil {
        log.Fatalf("Init Pool Error: %s", err)
    }
    conn1, err := pool.loan()
    if err != nil {
        log.Fatalf("Pool Loan Error: %s", err)
    }
    conn2, err := pool.loan()
    if err != nil {
        log.Fatalf("Pool Loan Error: %s", err)
    }
    pool.receive(conn1)
    pool.receive(conn2)
}
```

**Output:**

```
Loan Pool Object with ID: 0
Loan Pool Object with ID: 1
Return Pool Object with ID: 0
Return Pool Object with ID: 1
```

## **Full Working Code:**

```
package main

import (
    "fmt"
    "log"
    "strconv"
    "sync"
)

type iPoolObject interface {
    getID() string //This is any id which can be used to compare two different pool objects
}

type pool struct {
    idle   []iPoolObject
    active []iPoolObject
    capacity int
    mulock   *sync.Mutex
}

//InitPool Initialize the pool
func initPool(poolObjects []iPoolObject) (*pool, error) {
    if len(poolObjects) == 0 {
        return nil, fmt.Errorf("Cannot craete a pool of 0 length")
    }
    active := make([]iPoolObject, 0)
    pool := &pool{
        idle:     poolObjects,
        active:   active,
        capacity: len(poolObjects),
        mulock:   new(sync.Mutex),
    }
    return pool, nil
}

func (p *pool) loan() (iPoolObject, error) {
    p.mulock.Lock()
    defer p.mulock.Unlock()
    if len(p.idle) == 0 {
        return nil, fmt.Errorf("No pool object free. Please request after sometime")
    }
    obj := p.idle[0]
    p.idle = p.idle[1:]
    p.active = append(p.active, obj)
    fmt.Printf("Loan Pool Object with ID: %s\n", obj.getID())
    return obj, nil
}

func (p *pool) receive(target iPoolObject) error {
    p.mulock.Lock()
    defer p.mulock.Unlock()
    err := p.remove(target)
    if err != nil {
        return err
    }
    p.idle = append(p.idle, target)
    fmt.Printf("Return Pool Object with ID: %s\n", target.getID())
    return nil
}

func (p *pool) remove(target iPoolObject) error {
    currentActiveLength := len(p.active)
    for i, obj := range p.active {
        if obj.getID() == target.getID() {
            p.active[currentActiveLength-1], p.active[i] = p.active[i], p.active[currentActiveLength-1]
            p.active = p.active[:currentActiveLength-1]
            return nil
        }
    }
    return fmt.Errorf("Targe pool object doesn't belong to the pool")
}

type connection struct {
    id string
}

func (c *connection) getID() string {
    return c.id
}

func main() {
    connections := make([]iPoolObject, 0)
    for i := 0; i < 3; i++ {
        c := &connection{id: strconv.Itoa(i)}
        connections = append(connections, c)
    }
    pool, err := initPool(connections)
    if err != nil {
        log.Fatalf("Init Pool Error: %s", err)
    }
    conn1, err := pool.loan()
    if err != nil {
        log.Fatalf("Pool Loan Error: %s", err)
    }
    conn2, err := pool.loan()
    if err != nil {
        log.Fatalf("Pool Loan Error: %s", err)
    }
    pool.receive(conn1)
    pool.receive(conn2)
}
```

**Output:**

```
Loan Pool Object with ID: 0
Loan Pool Object with ID: 1
Return Pool Object with ID: 0
Return Pool Object with ID: 1
```*