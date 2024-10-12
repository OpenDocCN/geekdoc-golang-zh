<!--yml
category: 未分类
date: 2024-10-13 06:00:21
-->

# Golang: Redis client example

> 来源：[https://golangbyexample.com/golang-redis-client-example/](https://golangbyexample.com/golang-redis-client-example/)

First, we will write the Redis layer that will have a function to initialize the Redis client. The Redis client will only be created once and used throughout. In the below code, initialize function will initialize the Redis.

```
package main

import (
	"encoding/json"
	"time"
	"github.com/go-redis/redis"
)

var (
	client = &redisClient{}
)

type redisClient struct {
	c *redis.Client
}

//GetClient get the redis client
func initialize() *redisClient {
	c := redis.NewClient(&redis.Options{
		Addr: "127.0.0.1:6379",
	})

	if err := c.Ping().Err(); err != nil {
		panic("Unable to connect to redis " + err.Error())
	}
	client.c = c
	return client
}

//GetKey get key
func (client *redisClient) getKey(key string, src interface{}) error {
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
func (client *redisClient) setKey(key string, value interface{}, expiration time.Duration) error {
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

Test the Redis client layer

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
	redisClient := initialize()
	key1 := "sampleKey"
	value1 := &valueEx{Name: "someName", Email: "someemail@abc.com"}
	err := redisClient.setKey(key1, value1, time.Minute*1)
	if err != nil {
		log.Fatalf("Error: %v", err.Error())
	}

	value2 := &valueEx{}
	err = redisClient.getKey(key1, value2)
	if err != nil {
		log.Fatalf("Error: %v", err.Error())
	}

	log.Printf("Name: %s", value2.Name)
	log.Printf("Email: %s", value2.Email)
} 
```

**Output:**

```
Name: someName
Email: someemail@abc.com 
```