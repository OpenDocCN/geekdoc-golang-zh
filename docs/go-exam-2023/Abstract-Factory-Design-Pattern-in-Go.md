<!--yml

分类：未分类

日期：2024-10-13 06:01:47

-->

# Go中的抽象工厂设计模式

> 来源：[https://golangbyexample.com/abstract-factory-design-pattern-go/](https://golangbyexample.com/abstract-factory-design-pattern-go/)

注意：如果你对如何在GO中实现所有其他设计模式感兴趣，请查看这个完整参考 – [Go中的所有设计模式 (Golang)](https://golangbyexample.com/all-design-patterns-golang/)

目录

**   [定义：](#Definition "Definition:")

+   [代码：](#Code "Code:")*  *## **定义：**

抽象工厂设计模式是一种创建型设计模式，它允许你创建一组相关的对象。它是工厂模式的抽象化。最好用一个例子来解释。假设我们有两个工厂

+   **nike**

+   **adidas**

想象一下，你需要购买一个包含**鞋**和**短裤**的运动装备。通常情况下，你会想要购买一个同一工厂的完整运动装备，即**nike**或**adidas**。这就是抽象工厂发挥作用的地方，因为你想要的具体产品是**鞋**和**短裤**，这些产品将由**nike**和**adidas**的抽象工厂创建。

这两个工厂——**nike**和**adidas**实现了**iSportsFactory**接口。

我们有两个产品接口。

+   **iShoe** – 该接口由**nikeShoe**和**adidasShoe**具体产品实现。

+   **iShort** – 该接口由**nikeShort**和**adidasShort**具体产品实现。

现在让我们看看代码

## **代码：**

**iSportsFactory.go**

```go
package main

import "fmt"

type iSportsFactory interface {
    makeShoe() iShoe
    makeShort() iShort
}

func getSportsFactory(brand string) (iSportsFactory, error) {
    if brand == "adidas" {
        return &adidas{}, nil
    }
    if brand == "nike" {
        return &nike{}, nil
    }
    return nil, fmt.Errorf("Wrong brand type passed")
}
```

**adidas.go**

```go
package main

type adidas struct {
}

func (a *adidas) makeShoe() iShoe {
    return &adidasShoe{
        shoe: shoe{
            logo: "adidas",
            size: 14,
        },
    }
}

func (a *adidas) makeShort() iShort {
    return &adidasShort{
        short: short{
            logo: "adidas",
            size: 14,
        },
    }
} 
```

**nike.go**

```go
package main

type nike struct {
}

func (n *nike) makeShoe() iShoe {
    return &nikeShoe{
        shoe: shoe{
            logo: "nike",
            size: 14,
        },
    }
}

func (n *nike) makeShort() iShort {
    return &nikeShort{
        short: short{
            logo: "nike",
            size: 14,
        },
    }
}
```

**iShoe.go**

```go
package main

type iShoe interface {
    setLogo(logo string)
    setSize(size int)
    getLogo() string
    getSize() int
}

type shoe struct {
    logo string
    size int
}

func (s *shoe) setLogo(logo string) {
    s.logo = logo
}

func (s *shoe) getLogo() string {
    return s.logo
}

func (s *shoe) setSize(size int) {
    s.size = size
}

func (s *shoe) getSize() int {
    return s.size
} 
```

**adidasShoe.go**

```go
package main

type adidasShoe struct {
	shoe
} 
```

**nikeShoe.go**

```go
package main

type nikeShoe struct {
    shoe
}
```

**iShort.go**

```go
package main

type iShort interface {
    setLogo(logo string)
    setSize(size int)
    getLogo() string
    getSize() int
}

type short struct {
    logo string
    size int
}

func (s *short) setLogo(logo string) {
    s.logo = logo
}

func (s *short) getLogo() string {
    return s.logo
}

func (s *short) setSize(size int) {
    s.size = size
}

func (s *short) getSize() int {
    return s.size
}
```

**adidasShort.go**

```go
package main

type adidasShort struct {
    short
}
```

**nikeShort.go**

```go
package main

type nikeShort struct {
    short
}
```

**main.go**

```go
package main

import "fmt"

func main() {
    adidasFactory, _ := getSportsFactory("adidas")
    nikeFactory, _ := getSportsFactory("nike")
    nikeShoe := nikeFactory.makeShoe()
    nikeShort := nikeFactory.makeShort()
    adidasShoe := adidasFactory.makeShoe()
    adidasShort := adidasFactory.makeShort()
    printShoeDetails(nikeShoe)
    printShortDetails(nikeShort)
    printShoeDetails(adidasShoe)
    printShortDetails(adidasShort)
}

func printShoeDetails(s iShoe) {
    fmt.Printf("Logo: %s", s.getLogo())
    fmt.Println()
    fmt.Printf("Size: %d", s.getSize())
    fmt.Println()
}

func printShortDetails(s iShort) {
    fmt.Printf("Logo: %s", s.getLogo())
    fmt.Println()
    fmt.Printf("Size: %d", s.getSize())
    fmt.Println()
}
```

**输出：**

```go
Logo: nike
Size: 14
Logo: nike
Size: 14
Logo: adidas
Size: 14
Logo: adidas
Size: 14
```*
