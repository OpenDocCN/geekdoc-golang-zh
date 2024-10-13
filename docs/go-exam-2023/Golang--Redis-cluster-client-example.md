<!--yml

分类: 未分类

日期: 2024-10-13 06:02:16

-->

# Golang: Redis 集群客户端示例

> 来源：[https://golangbyexample.com/golang-redis-cluster-client-example/](https://golangbyexample.com/golang-redis-cluster-client-example/)

首先，我们将编写 Redis 集群层，该层将具有初始化 Redis 集群客户端的功能。Redis 集群客户端只会创建一次，并在整个过程中使用。在下面的代码中，初始化函数将初始化 Redis。

**client.go**

```go
package main

import (
    "encoding/json"
    "strings"
    "time"
    "github.com/go-redis/redis"
)

var (
    client = &redisCluterClient{}
)

//RedisClusterClient struct
type redisCluterClient struct {
    c *redis.ClusterClient
}

//GetClient get the redis client
func initialize(hostnames string) *redisCluterClient {
    addr := strings.Split(hostnames, ",")
    c := redis.NewClusterClient(&redis.ClusterOptions{
        Addrs: addr,
    })
    if err := c.Ping().Err(); err != nil {
        panic("Unable to connect to redis " + err.Error())
    }
    client.c = c
    return client
}

//GetKey get key
func (client *redisCluterClient) getKey(key string, src interface{}) error {
    val, err := client.c.Get(key).Result()
    if err == redis.Nil || err != nil {
        return err
    }
    err = json.Unmarshal([]byte(val), &src)
    if err != nil {
        return err
    }
    return nil
}

//SetKey set key
func (client *redisCluterClient) setKey(key string, value interface{}, expiration time.Duration) error {
    cacheEntry, err := json.Marshal(value)
    if err != nil {
        return err
    }
    err = client.c.Set(key, cacheEntry, expiration).Err()
    if err != nil {
        return err
    }
    return nil
}
```

**main.go**

```go
package main

import (
    "log"
    "time"
)

type valueEx struct {
    Name  string
    Email string
}

func main() {
    //Use your actually ip address here
    redisCluterClient := initialize("10.9.8.122:6379,10.9.8.123:6379,10.9.8.123:6379,")
    key1 := "sampleKey"
    value1 := &valueEx{Name: "someName", Email: "someemail@abc.com"}
    err := redisCluterClient.setKey(key1, value1, time.Minute*1)
    if err != nil {
        log.Fatalf("Error: %v", err.Error())
    }
    value2 := &valueEx{}
    err = redisCluterClient.getKey(key1, value2)
    if err != nil {
        log.Fatalf("Error: %v", err.Error())
    }
    log.Printf("Name: %s", value2.Name)
    log.Printf("Email: %s", value2.Email)
}
```

**输出**:

```go
Name: someName 
Email: someemail@abc.com
```
