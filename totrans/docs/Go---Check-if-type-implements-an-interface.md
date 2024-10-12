<!--yml
category: 未分类
date: 2024-10-13 06:05:08
-->

# Go : Check if type implements an interface

> 来源：[https://golangbyexample.com/go-check-if-type-implements-interface/](https://golangbyexample.com/go-check-if-type-implements-interface/)

Sometimes there can be scenarios where it is needed to know if your type satisfies an interface or not. This can be easily achieved using a blank identifier.

```
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

In the above program, we have commented out the square’s **getArea()** function. On “go build” above program gives the error

```
main.go:25:6: cannot use square literal (type square) as type shape in assignment:
        square does not implement shape (missing getArea method)
```

On uncommenting the **getArea()** method the error goes away. To check if a pointer of the type implements the interface then it can be done as below

```
var _ shape = &square{}
```

**OR**

```
var _ shape = (*square)(nil)
```

First one will give the error

```
main.go:25:6: cannot use (*square)(nil) (type *square) as type shape in assignment:
        *square does not implement shape (missing getArea method)
```

while the latter one will give error

```
main.go:25:6: cannot use &square literal (type *square) as type shape in assignment:
        *square does not implement shape (missing getArea method)
```

*   [go](https://golangbyexample.com/tag/go/)*   [interface](https://golangbyexample.com/tag/interface/)*   [type](https://golangbyexample.com/tag/type/)