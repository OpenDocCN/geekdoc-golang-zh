<!--yml

类别：未分类

日期：2024-10-13 06:02:04

-->

# 在Go（Golang）中迭代映射的不同方式

> 来源：[https://golangbyexample.com/different-ways-iterating-over-map-go/](https://golangbyexample.com/different-ways-iterating-over-map-go/)

范围操作符可用于在Go中迭代映射

首先定义一个映射

```
sample := map[string]string{
        "a": "x",
        "b": "y",
}
```

+   **迭代所有键和值**

```
for k, v := range sample {
   fmt.Printf("key :%s value: %s\n", k, v)
}
```

**输出：**

```
key :a value: x
key :b value: y
```

+   **仅迭代键**

```
for k := range sample {
   fmt.Printf("key :%s\n", k)
}
```

**输出：**

```
key :a
key :b
```

+   **仅迭代值**

```
for _, v := range sample {
   fmt.Printf("value :%s\n", v)
}
```

**输出：**

```
value :x
value :y
```

+   **获取所有键的列表**

```
keys := getAllKeys(sample)
fmt.Println(keys)

func getAllKeys(sample map[string]string) []string {
    var keys []string
    for k := range sample {
        keys = append(keys, k)
    }
    return keys
}
```

**输出：**

```
[a b]
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [迭代](https://golangbyexample.com/tag/iteration/)
