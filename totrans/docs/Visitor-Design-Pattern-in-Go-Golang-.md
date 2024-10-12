<!--yml
category: 未分类
date: 2024-10-13 06:04:34
-->

# Visitor Design Pattern in Go(Golang)

> 来源：[https://golangbyexample.com/visitor-design-pattern-go/](https://golangbyexample.com/visitor-design-pattern-go/)

Note: Interested in understanding how all other design patterns can be implemented in GO. Please see this full reference – [All Design Patterns in Go (Golang)](https://golangbyexample.com/all-design-patterns-golang/)

Table of Contents

 **   [Introduction:](#Introduction "Introduction:")
*   [UML Diagram:](#UML_Diagram "UML Diagram:")
*   [Mapping ](#Mapping "Mapping ")
*   [Example](#Example "Example")
*   [Full Working Code:](#Full_Working_Code "Full Working Code:")*  *# **Introduction:**

Visitor Design Pattern is a Behavioural design pattern that lets you add behaviour to a struct without actually modifying the struct.
Let’s understand the visitor pattern with an example. Let’s say you are the maintainer of a lib which has different shape structs such as

1.  Square
2.  Circle
3.  Triangle

Each of the above shape structs implements a common interface **shape.** There are many teams in your company which are using your lib. Now let’s say one of the team wants you to add one more behaviour (**getArea()**) to the Shape structs.

There are many options to solve this problem

**First Option**

The first option that comes to the mind is to add **getArea()** method in the **shape** interface and then each shape struct can implement the getArea() method. This seems trivial but there are some problems:

*   As a maintainer of the library, you don’t want to alter a highly tested code of your library by adding additional behaviours.
*   There might be more request by the teams using your library for more behaviours such as **getNumSides()**, **getMiddleCoordinates()**. Then, in this case, you don’t want to keep modifying your library. But you do want other teams to extend your library without actual modification of the code much.

**Second Option**

The second option is that the team requesting the feature can write the logic for behaviour themselves. So based upon the shape struct type they like below code

```
if shape.type == square {
   //Calculate area for squre
} elseif shape.type == circle {
    //Calculate area of triangle 
} elseif shape.type == "triangle" {
    //Calculate area of triangle
} else {
   //Raise error
} 
```

Above code is also problematic as you are not able to take the full advantage of interfaces and instead do an explicit type checking which is fragile. Second, getting the type at run time may have a performance impact or maybe even not possible in some languages.

**Third Option**

The third option is to solve the above problem using the visitor pattern. We define a visitor interface like below

```
type visitor interface {
   visitForSquare(square)
   visitForCircle(circle)
   visitForTriangle(triangle)
}
```

The functions **visitforSquare(square), visitForCircle(circle), visitForTriangle**(**triangle**) allows us to add functionality to Square, Circle and Triangle respectively.

Now the question which comes to mind is why can’t we have a single method visit**(shape)** in the visitor interface. The reason we don’t have it because GO and also some other languages support method overloading. So a different method for each of the struct.

We add an **accept** method to the shape interface with below signature and each of the shape struct needs to define this method.

```
func accept(v visitor)
```

But wait for a second, we just mentioned that we don’t want to modify our existing shape structs. But when using Visitor Pattern we do have to modify our shape structs but this modification will only be done once. In case adding any additional behaviour such as **getNumSides()**, **getMiddleCoordinates()** will use the same above **accept(v visitor)** function without any further change to the shape structs. Basically the shape structs just need to be modified once and all future request of additional behaviours will be handled using the same accept function. Let’s see how.

The square struct will implement an accept method like below:

```
func (obj *squre) accept(v visitor){
    v.visitForSquare(obj)
}
```

and similarly, circle and triangle will also define an accept function as above.

Now the team requesting for the **getArea()** behaviour can simply define the concrete implementation of visitor interface and write the area calculation logic in that concrete implementation.

**areaCalculator.go**

```
type areaCalculator struct{
    area int
}

func (a *areaCalculator) visitForSquare(s *square){
    //Calculate are for square
}
func (a *areaCalculator) visitForCircle(s *square){
    //Calculate are for circle
}
func (a *areaCalculator) visitForTriangle(s *square){
    //Calculate are for triangle
}
```

To calculate the area of a square we first create an instance of the square they can simply call.

```
sq := &square{}
ac := &areaCalculator{}
sq.accept(ac)
```

Similarly, other team requesting for **getMiddleCoordinates()** behaviour can define another concrete implementation of the visitor interface similar to above.

**middleCoordinates.go**

```
type middleCoordinates struct {
    x int
    y int
}

func (a *middleCoordinates) visitForSquare(s *square) {
    //Calculate middle point coordinates for square. After calculating the area assign in to the x and y instance variable.
}

func (a *middleCoordinates) visitForCircle(c *circle) {
    //Calculate middle point coordinates for square. After calculating the area assign in to the x and y instance variable.
}

func (a *middleCoordinates) visitForTriangle(t *triangle) {
    //Calculate middle point coordinates for square. After calculating the area assign in to the x and y instance variable.
}
```

# **UML Diagram:**

![](img/d728a8b95698322fae7c9e447f3d08a8.png)

Below is the corresponding mapping UML diagram with the practical example of shape struct and areaCalculator we gave above

![](img/76c1c67ce396b6579fea7d6a93abd60b.png)

# **Mapping **

The below table represents the mapping from the UML diagram actors to actual implementation actors in **“Example”** below

| element | shape.go |
| Concrete Element A | square.go |
| Concrete Element B | circle.go |
| Concrete Element C | rectangle.go |
| Visitor | visitor.go |
| Concrete Visitor 1 | areaCalculator.go |
| Concrete Visitor 2 | middleCoordinates.go |
| Client | main.go |

# **Example**

**shape.go**

```
package main

type shape interface {
    getType() string
    accept(visitor)
}
```

**square.go**

```
package main

type square struct {
    side int
}

func (s *square) accept(v visitor) {
    v.visitForSquare(s)
}

func (s *square) getType() string {
    return "Square"
}
```

**circle.go**

```
package main

type circle struct {
    radius int
}

func (c *circle) accept(v visitor) {
    v.visitForCircle(c)
}

func (c *circle) getType() string {
    return "Circle"
}
```

**rectangle.go**

```
package main

type rectangle struct {
    l int
    b int
}

func (t *rectangle) accept(v visitor) {
    v.visitForrectangle(t)
}

func (t *rectangle) getType() string {
    return "rectangle"
}
```

**visitor.go**

```
package main

type visitor interface {
    visitForSquare(*square)
    visitForCircle(*circle)
    visitForrectangle(*rectangle)
}
```

**areaCalculator.go**

```
package main

import (
    "fmt"
)

type areaCalculator struct {
    area int
}

func (a *areaCalculator) visitForSquare(s *square) {
    //Calculate area for square. After calculating the area assign in to the area instance variable
    fmt.Println("Calculating area for square")
}

func (a *areaCalculator) visitForCircle(s *circle) {
    //Calculate are for circle. After calculating the area assign in to the area instance variable
    fmt.Println("Calculating area for circle")
}

func (a *areaCalculator) visitForrectangle(s *rectangle) {
    //Calculate are for rectangle. After calculating the area assign in to the area instance variable
    fmt.Println("Calculating area for rectangle")
}
```

**middleCoordinates.go**

```
package main

import "fmt"

type middleCoordinates struct {
    x int
    y int
}

func (a *middleCoordinates) visitForSquare(s *square) {
    //Calculate middle point coordinates for square. After calculating the area assign in to the x and y instance variable.
    fmt.Println("Calculating middle point coordinates for square")
}

func (a *middleCoordinates) visitForCircle(c *circle) {
    //Calculate middle point coordinates for square. After calculating the area assign in to the x and y instance variable.
    fmt.Println("Calculating middle point coordinates for circle")
}

func (a *middleCoordinates) visitForrectangle(t *rectangle) {
    //Calculate middle point coordinates for square. After calculating the area assign in to the x and y instance variable.
    fmt.Println("Calculating middle point coordinates for rectangle")
}
```

**main.go**

```
package main

import "fmt"

func main() {
    square := &square{side: 2}
    circle := &circle{radius: 3}
    rectangle := &rectangle{l: 2, b: 3}

    areaCalculator := &areaCalculator{}
    square.accept(areaCalculator)
    circle.accept(areaCalculator)
    rectangle.accept(areaCalculator)

    fmt.Println()
    middleCoordinates := &middleCoordinates{}
    square.accept(middleCoordinates)
    circle.accept(middleCoordinates)
    rectangle.accept(middleCoordinates)
}
```

**Output:**

```
Calculating area for square
Calculating area for circle
Calculating area for rectangle

Calculating middle point coordinates for square
Calculating middle point coordinates for circle
Calculating middle point coordinates for rectangle
```

# **Full Working Code:**

```
package main

import "fmt"

type shape interface {
    getType() string
    accept(visitor)
}

type square struct {
    side int
}

func (s *square) accept(v visitor) {
    v.visitForSquare(s)
}

func (s *square) getType() string {
    return "Square"
}

type circle struct {
    radius int
}

func (c *circle) accept(v visitor) {
    v.visitForCircle(c)
}

func (c *circle) getType() string {
    return "Circle"
}

type rectangle struct {
    l int
    b int
}

func (t *rectangle) accept(v visitor) {
    v.visitForrectangle(t)
}

func (t *rectangle) getType() string {
    return "rectangle"
}

type visitor interface {
    visitForSquare(*square)
    visitForCircle(*circle)
    visitForrectangle(*rectangle)
}

type areaCalculator struct {
    area int
}

func (a *areaCalculator) visitForSquare(s *square) {
    //Calculate area for square. After calculating the area assign in to the area instance variable
    fmt.Println("Calculating area for square")
}

func (a *areaCalculator) visitForCircle(s *circle) {
    //Calculate are for circle. After calculating the area assign in to the area instance variable
    fmt.Println("Calculating area for circle")
}

func (a *areaCalculator) visitForrectangle(s *rectangle) {
    //Calculate are for rectangle. After calculating the area assign in to the area instance variable
    fmt.Println("Calculating area for rectangle")
}

type middleCoordinates struct {
    x int
    y int
}

func (a *middleCoordinates) visitForSquare(s *square) {
    //Calculate middle point coordinates for square. After calculating the area assign in to the x and y instance variable.
    fmt.Println("Calculating middle point coordinates for square")
}

func (a *middleCoordinates) visitForCircle(c *circle) {
    //Calculate middle point coordinates for square. After calculating the area assign in to the x and y instance variable.
    fmt.Println("Calculating middle point coordinates for circle")
}

func (a *middleCoordinates) visitForrectangle(t *rectangle) {
    //Calculate middle point coordinates for square. After calculating the area assign in to the x and y instance variable.
    fmt.Println("Calculating middle point coordinates for rectangle")
}

func main() {
    square := &square{side: 2}
    circle := &circle{radius: 3}
    rectangle := &rectangle{l: 2, b: 3}
    areaCalculator := &areaCalculator{}
    square.accept(areaCalculator)
    circle.accept(areaCalculator)
    rectangle.accept(areaCalculator)

    fmt.Println()
    middleCoordinates := &middleCoordinates{}
    square.accept(middleCoordinates)
    circle.accept(middleCoordinates)
    rectangle.accept(middleCoordinates)
}
```

**Output:**

```
Calculating area for square
Calculating area for circle
Calculating area for rectangle

Calculating middle point coordinates for square
Calculating middle point coordinates for circle
Calculating middle point coordinates for rectangle
```

*   [design](https://golangbyexample.com/tag/design/)*   [go](https://golangbyexample.com/tag/go/)*   [visitor](https://golangbyexample.com/tag/visitor/)*