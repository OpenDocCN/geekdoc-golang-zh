<!--yml

category: 未分类

date: 2024-10-13 06:21:22

-->

# Go（Golang）中的变量作用域

> 来源：[`golangbyexample.com/scope-of-variable-go/`](https://golangbyexample.com/scope-of-variable-go/)

**变量的作用域（局部变量和全局变量）**

变量声明可以在包级、函数级或块级进行。变量的作用域定义了从哪里可以访问该变量，以及变量的生命周期。Golang 变量可以根据作用域分为两类。

+   局部变量

+   全局变量

**局部变量：**

+   局部变量是在块或函数级别定义的变量。

+   块的一个例子是 for 循环或范围循环等。

+   这些变量只能在其块或函数内部访问。

+   这些变量仅在声明它们的块或函数结束之前存在。之后，它们将被垃圾回收。

+   一旦声明的局部变量不能在同一块或函数内重新声明。

请见下面的例子。

+   循环变量**i**在 for 循环后不可用。

+   同样，局部变量**aaa**在声明它的函数之外不可用。

因此，下面的程序将引发编译错误。

```go
undefined: i
undefined: aaa.  #this occurs in the testLocal() function
```

**代码：**

```go
package main

import "fmt"

func main() {
    var aaa = "test"
    fmt.Println(aaa)
    for i := 0; i < 3; i++ {
        fmt.Println(i)
    }
    fmt.Println(i)
}

func testLocal() {
    fmt.Println(aaa)
}
```

**全局变量**

+   如果在文件顶部声明了一个变量，而不在任何函数或块的作用域内，则该变量在包内是全局的。

+   如果变量名以小写字母开头，则可以在包含该变量定义的包内访问。

+   如果变量名以大写字母开头，则可以从声明它的不同包之外访问。

+   全局变量在程序的生命周期内是可用的。

例如，在下面的程序中，变量 aaa 将是一个在主包中可用的全局变量。它将在**main**包内的任何函数中可用。请注意，由于变量名以小写字母开头，因此该变量名在**main**包外不可用。

```go
package main

import "fmt"

var aaa = "test"

func main() {
    testGlobal()
}

func testGlobal() {
    fmt.Println(aaa)
}
```

**输出：**

```go
test
```

重要事项

+   在内部作用域声明的变量，如果与外部作用域中的变量同名，将会遮蔽外部作用域中的变量。

```go
package main

import "fmt"

var a = 123

func main() {
    var a = 456
    fmt.Println(a)
}
```

**输出：**

```go
456
```


