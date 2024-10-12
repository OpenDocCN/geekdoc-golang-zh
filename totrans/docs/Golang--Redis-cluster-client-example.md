<!--yml
category: 未分类
date: 2024-10-13 06:02:16
-->

# Golang: Redis cluster client example

> 来源：[https://golangbyexample.com/golang-redis-cluster-client-example/](https://golangbyexample.com/golang-redis-cluster-client-example/)

First, we will write the Redis cluster layer that will have a function to initialize the Redis cluster client. The Redis cluster client will only be created once and used throughout. In the below code, initialize function will initialize the Redis.

**client.go**

```
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

```
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

**Output**:

```
Name: someName 
Email: someemail@abc.com
```