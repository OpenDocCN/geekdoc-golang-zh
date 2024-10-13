<!--yml

类别：未分类

日期：2024-10-13 06:04:10

-->

# Go 中的对象池设计模式

> 来源：[`golangbyexample.com/golang-object-pool/`](https://golangbyexample.com/golang-object-pool/)

注意：如果想了解如何在 GO 中实现所有其他设计模式，请查看这个完整参考 – [Go 中的所有设计模式](https://golangbyexample.com/all-design-patterns-golang/)

目录

**   介绍：

+   何时使用：

+   示例：

+   完整工作代码：

## **介绍：**

对象池设计模式是一种创建型设计模式，其中一个对象池在之前被初始化和创建，并保持在池中。根据需要，客户端可以从池中请求一个对象，使用它，然后将其返回到池中。池中的对象从未被销毁。

## **何时使用：**

+   当创建类的对象成本高，而在特定时间所需的此类对象数量不多时。

-让我们以数据库连接为例。每个连接对象的创建成本很高，因为涉及网络调用，而且在某个时刻不需要超过一定数量的连接。对象池设计模式非常适合这种情况。

+   当池对象是不可变对象时。

-再以数据库连接为例。数据库连接是一个不可变对象。几乎没有其属性需要改变。

+   出于性能原因。它会显著提高应用程序的性能，因为池已经创建好了。

## **示例：**

**iPoolObject.go**

```go
package main

type iPoolObject interface {
    getID() string //This is any id which can be used to compare two different pool objects
}
```

**pool.go**

```go
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

```go
package main

type connection struct {
    id string
}

func (c *connection) getID() string {
    return c.id
}
```

**main.go**

```go
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

**输出：**

```go
Loan Pool Object with ID: 0
Loan Pool Object with ID: 1
Return Pool Object with ID: 0
Return Pool Object with ID: 1
```

## **完整工作代码：**

```go
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

**输出：**

```go
Loan Pool Object with ID: 0
Loan Pool Object with ID: 1
Return Pool Object with ID: 0
Return Pool Object with ID: 1
```*
