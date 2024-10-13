<!--yml

category: 未分类

date: 2024-10-13 06:19:34

-->

# Go中结构体的导出与未导出字段

> 来源：[https://golangbyexample.com/exported-unexported-fields-struct-go/](https://golangbyexample.com/exported-unexported-fields-struct-go/)

Go没有任何公共、私有或受保护的关键字。控制包外可见性的唯一机制是使用大写和小写格式。

+   **大写标识符** 是导出的。大写字母表示这是一个导出的标识符，可以在包外使用。

+   **小写标识符** 是未导出的。小写字母表示该标识符未导出，只能在同一包内访问。

因此，任何以大写字母开头的结构体都被导出到其他包。同样，任何以大写字母开头的结构体字段也是导出的，反之则不是。任何以大写字母开头的结构体方法也是导出的。让我们看一个示例，展示结构体、结构体字段和方法的导出与未导出。请见下面的 **model.go** 和 **test.go**，它们都属于 **main** 包。

+   结构体

    +   结构体 **Person** 是导出的

    +   结构体 **company** 是未导出的

+   结构体的字段

    +   **Person** 结构体字段 **Name** 是导出的

    +   **Person** 结构体字段 **age** 是未导出的，但 **Name** 是导出的

+   结构体的方法

    +   **Person** 结构体的方法 **GetAge()** 是导出的

    +   **Person** 结构体的方法 **getName()** 是未导出的

**model.go**

```go
package main

import "fmt"

//Person struct
type Person struct {
    Name string
    age  int
}

//GetAge of person
func (p *Person) GetAge() int {
    return p.age
}

func (p *Person) getName() string {
    return p.Name
}

type company struct {
}
```

让我们在同一 **main** 包中编写一个文件 **test.go**。见下文。

**test.go**

```go
package main

import "fmt"

//Test function
func Test() {
    //STRUCTURE IDENTIFIER
    p := &Person{
        Name: "test",
        age:  21,
    }
    fmt.Println(p)
    c := &company{}
    fmt.Println(c)

    //STRUCTURE'S FIELDS
    fmt.Println(p.Name)
    fmt.Println(p.age)

    //STRUCTURE'S METHOD
    fmt.Println(p.GetAge())
    fmt.Println(p.getName())

}
```

运行此文件时，它能够访问 **model.go** 中的所有导出和未导出字段，因为它们都在同一包 **main** 中。没有编译错误，输出如下。

**输出:**

```go
&{test 21}
&{}
test
21
21
test
```

现在让我们将上述文件 **model.go** 移动到一个名为 **model** 的不同包中。现在注意运行‘go build’时的输出。它会给出编译错误。所有编译错误都是因为 **main** 包中的 **test.go** 无法引用 **model** 包中未导出的 **model.go** 字段。

**model.go**

```go
package model

//Person struct
type Person struct {
	Name string
	age  int
}

func (p *Person) GetAge() int {
    return p.age
}

func (p *Person) getName() string {
    return p.Name
}

type company struct {
}
```

**test.go**

```go
package main

import (
	"fmt"
        //This will path of your model package
	"<somepath>/model"
)

//Test function
func main() {
	//STRUCTURE IDENTIFIER
	p := &model.Person{
		Name: "test",
		age:  21,
	}
	fmt.Println(p)
	c := &model.company{}
	fmt.Println(c)

	//STRUCTURE'S FIELDS
	fmt.Println(p.Name)
	fmt.Println(p.age)

        //STRUCTURE'S METHOD
        fmt.Println(p.GetAge())
        fmt.Println(p.getName())

}</somepath>
```

**输出:**

```go
unknown field 'age' in struct literal of type model.Person
cannot refer to unexported name model.company
undefined: model.company
p.age undefined (cannot refer to unexported field or method age)
p.getName undefined (cannot refer to unexported field or method model.(*Person).getName)
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)
