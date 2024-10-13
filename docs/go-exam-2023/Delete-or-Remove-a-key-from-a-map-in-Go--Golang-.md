<!--yml

类别：未分类

日期：2024-10-13 06:52:35

-->

# 在 Go (Golang)中删除或移除映射中的键

> 来源：[`golangbyexample.com/delete-key-map-golang/`](https://golangbyexample.com/delete-key-map-golang/)

目录

+   概述

+   键存在于映射中

+   键在映射中不存在

# **概述**

以下是从映射中删除给定键的格式

```go
delete(mapName, keyToDelete)
```

有两个情况

+   **keyToDelete**在映射中存在。在这种情况下，它将简单地删除该键

+   **keyToDelete**在映射中不存在。在这种情况下，它将不执行任何操作。A

让我们看一下这两种情况的示例

# **键存在于映射中**

以下是键存在于映射中的程序

```go
package main

import "fmt"

func main() {
	sample := make(map[string]int)
	sample["a"] = 1
	sample["b"] = 2
	sample["c"] = 3

        fmt.Println(sample)
	delete(sample, "a")

	fmt.Println(sample)
}
```

**输出**

```go
map[a:1 b:2 c:3]
map[b:2 c:3]
```

注意输出中，键“a”已从映射中删除。检查键是否存在于映射中，然后再删除总是一个好习惯。以下是相关的代码片段。

```go
package main

import "fmt"

func main() {
	sample := make(map[string]int)
	sample["a"] = 1
	sample["b"] = 2
	sample["c"] = 3

	fmt.Println(sample)

	if _, ok := sample["a"]; ok {
		delete(sample, "a")
	}

	fmt.Println(sample)
}
```

**输出**

```go
map[a:1 b:2 c:3]
map[b:2 c:3]
```

# **键在映射中不存在**

在这种情况下，检查键是否存在然后再删除也是一个好习惯。即使我们直接删除而不检查，也不是问题。以下代码片段显示了这两种情况

```go
package main

import "fmt"

func main() {
	sample := make(map[string]int)
	sample["a"] = 1
	sample["b"] = 2
	sample["c"] = 3

	fmt.Println(sample)

	//Check and delete
	if _, ok := sample["d"]; ok {
		delete(sample, "d")
	}

	fmt.Println(sample)

	//Directly delete
	delete(sample, "d")

	fmt.Println(sample)
}
```

**输出**

```go
map[a:1 b:2 c:3]
map[a:1 b:2 c:3]
map[a:1 b:2 c:3]
```

**注意：** 请查看我们的 Golang 高级教程。本系列的教程内容详尽，我们尝试用示例覆盖所有概念。这个教程是为了那些希望获得 Golang 专业知识和扎实理解的人—[Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章就是为你准备的—

[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)*
