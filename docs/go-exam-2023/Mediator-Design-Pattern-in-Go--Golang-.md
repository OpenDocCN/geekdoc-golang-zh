<!--yml

分类：未分类

日期：2024-10-13 06:03:53

-->

# Go (Golang)中的中介设计模式

> 来源：[https://golangbyexample.com/mediator-design-pattern-golang/](https://golangbyexample.com/mediator-design-pattern-golang/)

注意：有兴趣了解如何在GO中实现所有其他设计模式，请查看此完整参考 – [Go中的所有设计模式](https://golangbyexample.com/all-design-patterns-golang/)

目录

**[介绍：](#Introduction "Introduction:")**

+   [实用示例](#Practical_Example "Practical Example")

+   [完整工作代码：](#Full_Working_Code "Full Working Code:")*  *## **介绍：**

中介设计模式是一种行为设计模式。该模式建议创建一个中介对象，以防止对象之间的直接通信，从而避免它们之间的直接依赖。

中介模式的一个很好的例子是铁路系统平台。两个火车之间不会直接通信以了解平台的可用性。**stationManager**充当中介，仅使平台对一列火车可用。火车与**stationManager**连接并相应地行动。它维护等待火车的队列。如果有任何火车离开平台，它会通知其中一列火车下次到达平台。

注意**stationManger**在下面代码中如何充当**火车**与**平台**之间的中介。

+   passengerTrain和goodsTrain实现了火车接口。

+   stationManger实现了中介接口。

## **实用示例**

**train.go**

```
package main

type train interface {
    requestArrival()
    departure()
    permitArrival()
}
```

**passengerTrain.go**

```
package main

import "fmt"

type passengerTrain struct {
    mediator mediator
}

func (g *passengerTrain) requestArrival() {
    if g.mediator.canLand(g) {
        fmt.Println("PassengerTrain: Landing")
    } else {
        fmt.Println("PassengerTrain: Waiting")
    }
}

func (g *passengerTrain) departure() {
    fmt.Println("PassengerTrain: Leaving")
    g.mediator.notifyFree()
}

func (g *passengerTrain) permitArrival() {
    fmt.Println("PassengerTrain: Arrival Permitted. Landing")
}
```

**goodsTrain.go**

```
package main

import "fmt"

type goodsTrain struct {
    mediator mediator
}

func (g *goodsTrain) requestArrival() {
    if g.mediator.canLand(g) {
        fmt.Println("GoodsTrain: Landing")
    } else {
        fmt.Println("GoodsTrain: Waiting")
    }
}

func (g *goodsTrain) departure() {
    g.mediator.notifyFree()
    fmt.Println("GoodsTrain: Leaving")
}

func (g *goodsTrain) permitArrival() {
    fmt.Println("GoodsTrain: Arrival Permitted. Landing")
}
```

**mediator.go**

```
package main

type mediator interface {
    canLand(train) bool
    notifyFree()
}
```

**stationManager.go**

```
package main

import "sync"

type stationManager struct {
    isPlatformFree bool
    lock           *sync.Mutex
    trainQueue     []train
}

func newStationManger() *stationManager {
    return &stationManager{
        isPlatformFree: true,
        lock:           &sync.Mutex{},
    }
}

func (s *stationManager) canLand(t train) bool {
    s.lock.Lock()
    defer s.lock.Unlock()
    if s.isPlatformFree {
        s.isPlatformFree = false
        return true
    }
    s.trainQueue = append(s.trainQueue, t)
    return false
}

func (s *stationManager) notifyFree() {
    s.lock.Lock()
    defer s.lock.Unlock()
    if !s.isPlatformFree {
        s.isPlatformFree = true
    }
    if len(s.trainQueue) > 0 {
        firstTrainInQueue := s.trainQueue[0]
        s.trainQueue = s.trainQueue[1:]
        firstTrainInQueue.permitArrival()
    }
}
```

**main.go**

```
package main

func main() {
    stationManager := newStationManger()
    passengerTrain := &passengerTrain{
        mediator: stationManager,
    }
    goodsTrain := &goodsTrain{
        mediator: stationManager,
    }
    passengerTrain.requestArrival()
    goodsTrain.requestArrival()
    passengerTrain.departure()
}
```

**输出：**

```
PassengerTrain: Landing
GoodsTrain: Waiting
PassengerTrain: Leaving
GoodsTrain: Arrival Permitted. Landing
```

## **完整工作代码：**

```
package main

import (
    "fmt"
    "sync"
)

type train interface {
    requestArrival()
    departure()
    permitArrival()
}

type passengerTrain struct {
    mediator mediator
}

func (g *passengerTrain) requestArrival() {
    if g.mediator.canLand(g) {
        fmt.Println("PassengerTrain: Landing")
    } else {
        fmt.Println("PassengerTrain: Waiting")
    }
}

func (g *passengerTrain) departure() {
    fmt.Println("PassengerTrain: Leaving")
    g.mediator.notifyFree()
}

func (g *passengerTrain) permitArrival() {
    fmt.Println("PassengerTrain: Arrival Permitted. Landing")
}

type goodsTrain struct {
    mediator mediator
}

func (g *goodsTrain) requestArrival() {
    if g.mediator.canLand(g) {
        fmt.Println("GoodsTrain: Landing")
    } else {
        fmt.Println("GoodsTrain: Waiting")
    }
}

func (g *goodsTrain) departure() {
    g.mediator.notifyFree()
    fmt.Println("GoodsTrain: Leaving")
}

func (g *goodsTrain) permitArrival() {
    fmt.Println("GoodsTrain: Arrival Permitted. Landing")
}

type mediator interface {
    canLand(train) bool
    notifyFree()
}

type stationManager struct {
    isPlatformFree bool
    lock           *sync.Mutex
    trainQueue     []train
}

func newStationManger() *stationManager {
    return &stationManager{
        isPlatformFree: true,
        lock:           &sync.Mutex{},
    }
}

func (s *stationManager) canLand(t train) bool {
    s.lock.Lock()
    defer s.lock.Unlock()
    if s.isPlatformFree {
        s.isPlatformFree = false
        return true
    }
    s.trainQueue = append(s.trainQueue, t)
    return false
}

func (s *stationManager) notifyFree() {
    s.lock.Lock()
    defer s.lock.Unlock()
    if !s.isPlatformFree {
        s.isPlatformFree = true
    }
    if len(s.trainQueue) > 0 {
        firstTrainInQueue := s.trainQueue[0]
        s.trainQueue = s.trainQueue[1:]
        firstTrainInQueue.permitArrival()
    }
}

func main() {
    stationManager := newStationManger()
    passengerTrain := &passengerTrain{
        mediator: stationManager,
    }
    goodsTrain := &goodsTrain{
        mediator: stationManager,
    }
    passengerTrain.requestArrival()
    goodsTrain.requestArrival()
    passengerTrain.departure()
}
```

**输出：**

```
PassengerTrain: Landing
GoodsTrain: Waiting
PassengerTrain: Leaving
GoodsTrain: Arrival Permitted. Landing
```

+   [设计模式](https://golangbyexample.com/tag/design-pattern/)*   [go](https://golangbyexample.com/tag/go/)*   [中介](https://golangbyexample.com/tag/mediator/)*
