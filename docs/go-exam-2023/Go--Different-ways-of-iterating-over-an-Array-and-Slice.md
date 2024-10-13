<!--yml

分类: 未分类

日期: 2024-10-13 06:01:59

-->

# Go: 遍历数组和切片的不同方式

> 来源：[`golangbyexample.com/go-different-ways-iterating-array-slice/`](https://golangbyexample.com/go-different-ways-iterating-array-slice/)

Go 提供了许多遍历数组的不同方式。以下所有示例也适用于切片。

让我们先定义一个**字母**的数组

```go
letters := []string{"a", "b", "c", "d", "e"}
```

目录

**   使用 range 操作符

+   仅使用 For 操作符*  *## **使用 range 操作符**

+   带索引和值

```go
for i, letter := range letters {
   fmt.Printf("%d %s\n", i, letter)
}
```

+   仅值

```go
for _, letter := range letters {
   fmt.Println(letter)
}
```

+   仅索引

```go
for i := range letters {
   fmt.Println(i)
}
```

+   无值和索引。仅打印数组值

```go
i := 0
for range letters {
  fmt.Println(i)
  i++
}
```

## **仅使用 For 操作符**

+   单一初始化和后置

```go
len := len(letters)
for i := 0; i < len; i++ {
  fmt.Println(letters[i])
}
```

+   多重初始化和后置语句

```go
len := len(letters)
for i, j := 0, len; i < j; i, j = i+1, j-1 {
  fmt.Println(letters[i])
}
```

+   [数组](https://golangbyexample.com/tag/array/)*   [Go](https://golangbyexample.com/tag/go/)*   [Golang](https://golangbyexample.com/tag/golang/)*   [遍历](https://golangbyexample.com/tag/iterating/)*   [迭代](https://golangbyexample.com/tag/iteration/)*   [切片](https://golangbyexample.com/tag/slice/)*
