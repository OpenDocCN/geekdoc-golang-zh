<!--yml
category: 未分类
date: 2024-10-13 06:02:04
-->

# Different ways of iterating over a map in Go (Golang)

> 来源：[https://golangbyexample.com/different-ways-iterating-over-map-go/](https://golangbyexample.com/different-ways-iterating-over-map-go/)

Range operator can be used to iterate over a map in Go

Let’s define a map first

```
sample := map[string]string{
        "a": "x",
        "b": "y",
}
```

*   **Iterating over all keys and values**

```
for k, v := range sample {
   fmt.Printf("key :%s value: %s\n", k, v)
}
```

**Output:**

```
key :a value: x
key :b value: y
```

*   **Iterating over only keys**

```
for k := range sample {
   fmt.Printf("key :%s\n", k)
}
```

**Output:**

```
key :a
key :b
```

*   **Iterating over only values**

```
for _, v := range sample {
   fmt.Printf("value :%s\n", v)
}
```

**Output:**

```
value :x
value :y
```

*   **Get list of all keys**

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

**Output:**

```
[a b]
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [iteration](https://golangbyexample.com/tag/iteration/)