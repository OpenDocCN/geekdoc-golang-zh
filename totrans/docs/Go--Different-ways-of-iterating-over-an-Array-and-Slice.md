<!--yml
category: 未分类
date: 2024-10-13 06:01:59
-->

# Go: Different ways of iterating over an Array and Slice

> 来源：[https://golangbyexample.com/go-different-ways-iterating-array-slice/](https://golangbyexample.com/go-different-ways-iterating-array-slice/)

Go provides many different ways of iterating over an array. All examples below are also applicable to slice.

Let’s define an array of **letters** first

```
letters := []string{"a", "b", "c", "d", "e"}
```

Table of Contents

 **   [Using the range operator](#Using_the_range_operator "Using the range operator")
*   [Using Only For operator](#Using_Only_For_operator "Using Only For operator")*  *## **Using the range operator**

*   With index and value

```
for i, letter := range letters {
   fmt.Printf("%d %s\n", i, letter)
}
```

*   Only Value

```
for _, letter := range letters {
   fmt.Println(letter)
}
```

*   Only index

```
for i := range letters {
   fmt.Println(i)
}
```

*   Without value and index. Just print array values

```
i := 0
for range letters {
  fmt.Println(i)
  i++
}
```

## **Using Only For operator**

*   Single initialization and post

```
len := len(letters)
for i := 0; i < len; i++ {
  fmt.Println(letters[i])
}
```

*   Multiple initialization and post statement

```
len := len(letters)
for i, j := 0, len; i < j; i, j = i+1, j-1 {
  fmt.Println(letters[i])
}
```

*   [array](https://golangbyexample.com/tag/array/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [iterating](https://golangbyexample.com/tag/iterating/)*   [iteration](https://golangbyexample.com/tag/iteration/)*   [slice](https://golangbyexample.com/tag/slice/)*