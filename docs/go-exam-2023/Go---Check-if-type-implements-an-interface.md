<!--yml

类别：未分类

日期：2024-10-13 06:05:08

-->

# Go : 检查类型是否实现接口

> 来源：[https://golangbyexample.com/go-check-if-type-implements-interface/](https://golangbyexample.com/go-check-if-type-implements-interface/)

有时可能会出现需要知道你的类型是否满足某个接口的场景。这可以通过使用空标识符轻松实现。

```go
package main

type shape interface {
    getNumSides() int
    getArea() int
}

type square struct {
    len int
}

func (s square) getNumSides() int {
    return 4
}

// func (s square) getArea() int {
//  return s.len * 2
// }

func main() {
    //Verify that *square implement shape
    var _ shape = square{}
}
```

在上面的程序中，我们注释掉了正方形的**getArea()**函数。在“go build”中，上述程序会给出错误

```go
main.go:25:6: cannot use square literal (type square) as type shape in assignment:
        square does not implement shape (missing getArea method)
```

取消注释**getArea()**方法后，错误消失了。要检查该类型的指针是否实现接口，可以如下进行

```go
var _ shape = &square{}
```

**或者**

```go
var _ shape = (*square)(nil)
```

第一个会导致错误

```go
main.go:25:6: cannot use (*square)(nil) (type *square) as type shape in assignment:
        *square does not implement shape (missing getArea method)
```

而后者会导致错误

```go
main.go:25:6: cannot use &square literal (type *square) as type shape in assignment:
        *square does not implement shape (missing getArea method)
```

+   [go](https://golangbyexample.com/tag/go/)*   [interface](https://golangbyexample.com/tag/interface/)*   [type](https://golangbyexample.com/tag/type/)
