<!--yml
category: 未分类
date: 2024-10-13 06:30:17
-->

# Init function in Go (Golang)

> 来源：[https://golangbyexample.com/init-function-golang/](https://golangbyexample.com/init-function-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Init function in the main package](#Init_function_in_the_main_package "Init function in the main package")
*   [Multiple init() function in different source file of same package](#Multiple_init_function_in_different_source_file_of_same_package "Multiple init() function in different source file of same package")
    *   [Multiple init() function in same source file](#Multiple_init_function_in_same_source_file "Multiple init() function in same source file")
*   [Order of execution of program](#Order_of_execution_of_program "Order of execution of program")*  *# **Overview**

init() function is a special function that is used to initialize global variables of a package. These functions are executed when the package is initialized. Each of the GO source files in a package can have its own init() function. Whenever you import any package in the program, then on the execution of that program, init functions(if present)  in the GO source files belonging to that imported package are called first. Some points to note about init function

*   Init function is optional

*   Init function does not take any argument

*   Init function does not have any return value.

*   Init function is called implicitly. Since it is called implicitly, init function cannot reference it from anywhere.

*   There can be multiple init() functions within the same source file.

init function is majorly used for the initialization of global variables that cannot be initialized using an initialization expression. For example, it requires a network call to intialize any DB client. Another example could be fetching secret keys on startup. Init function is also used for running anything that only needs to be executed once. Let’s see a simple use case of using an init function. 

Assume you are using redis cache in your application. You want redis client to created during the application first launch. A good place to initialize a redis client is in init() function. Let’s see an example.

Create a module named redisexample and put below files in that module.

**go.mod**

```
module redisexample

go 1.14
```

**redis/redis.go**

```
package redis

import (
	"encoding/json"
	"time"

	"github.com/go-redis/redis"
)

type redisClient struct {
	c *redis.Client
}

var (
	client = &redisClient{}
)

func init() {
	c := redis.NewClient(&redis.Options{
		Addr: "127.0.0.1:6379",
	})

	if err := c.Ping().Err(); err != nil {
		panic("Unable to connect to redis " + err.Error())
	}
	client.c = c
}

func GetKey(key string, src interface{}) error {
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
func SetKey(key string, value interface{}, expiration time.Duration) error {
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
	"fmt"
	"log"
	"time"

	"redisexample/redis"
)

func main() {
	err := redis.SetKey("a", "b", time.Minute*1)
	if err != nil {
		log.Fatalf("Error: %v", err.Error())
	}

	var value string
	err = redis.GetKey("a", &value)
	if err != nil {
		log.Fatalf("Error: %v", err.Error())
	}

	fmt.Println(value)

}
```

**Output**

```
b
```

In the above example, we have an init function in the **redis.go** file. In the init function we initialize the redis global client which is in turn used in the **main** function in main.go.

# **Init function in the main package**

**main** package can also contain the init function. The init function will be triggered before the **main** function

```
package main

import "fmt"

var sample int

func init() {
	fmt.Println("In Init function")
	sample = 4
}

func main() {
	fmt.Println("Main started")
	fmt.Println(sample)
	fmt.Println("Main Ended")
}
```

**Output**

```
In Init function
Main started
4
Main Ended
```

In the above program, we have an init function and the main function. In the init function, we initialize the global variable “sample”. Notice the order of execution i.e init function is executed first and then the main function is executed.

# **Multiple init() function in different source file of same package**

**init()** function is called in all the source files belonging to a package if present. Let’s see an example for it.

**learn/math/add.go**

```
package math

import "fmt"

func init(){
	fmt.Println("In add init")
}

func Add(a, b int) int {
	return a + b
}
```

**learn/math/subtract.go**

```
package math

import "fmt"

func init(){
	fmt.Println("In subtract init")
}

func Subtract(a, b int) int {
	return a - b
}
```

**learn/main.go**

```
package main

import (
	"fmt"

	"github.com/learn/math"
)

func main() {
	fmt.Println(math.Add(2, 1))
	fmt.Println(math.Subtract(2, 1))
}
```

**Output**

```
In add init
In subtract init
3
1
```

## **Multiple init() function in same source file**

There can be multiple init function within same source file.

```
package main

import "fmt"

var sample int

func init() {
	fmt.Println("Init1")
}

func init() {
	fmt.Println("Init2")
}

func main() {
	fmt.Println("Main started")
	fmt.Println("Main Ended")
}
```

**Output**

```
Init1
Init2
Main started
Main Ended
```

In above program we have an init function and a main function. In init function we initialize the global variable “sample”. Notice the order of execution i.e init function is executed first and then the main function is executed.

# **Order of execution of program**

Now as we have seen above examples, let’s see the order of execution of a go program.

*   The program starts with the main package.

*   All imported packages in the source files of the main package are initialized. The same thing happens recursively for further imported packages.

*   Then global variables declaration in these packages is initialized. The initialization dependency kicks in for initialization of these variables. [https://golang.org/ref/spec#Order_of_evaluation](https://golang.org/ref/spec#Order_of_evaluation)

*   After this, init() function is run in these packages

*   Global variables in the main package are initialized

*   init function in the main package is run if present

*   main function in main package is run.

Note here that package initialization is only done once even if it is imported several times.

For example if main package imports package **a** and in turn package **a** imports package **b**, then below will be the order

*   Global variables in package **b** will be initialized. init function in source files of package b will be run

*   Global variables in the package **a** will be initialized. init function in source files of package **b** will be run

*   Global variables in **main** package will be intialized. init function in source files of **main** package will be run

*   **main** function will start executing.

Let’s see a program for the same.

**go.mod**

```
module sample

go 1.14
```

**sample/b/b1.go**

```
package b

import (
	"fmt"
)

func init() {
	fmt.Println("Init: b1")
}

func TestB() error {
	return nil
}
```

**sample/b/b2.go**

```
package b

import (
	"fmt"
)

func init() {
	fmt.Println("Init: b2")
}
```

**sample/a/a1.go**

```
package a

import (
	"fmt"
	"sample/b"
)

func init() {
	fmt.Println("Init: a1")
}

func TestA() error {
	return b.TestB()
}
```

**sample/a/a2.go**

```
package a

import (
	"fmt"
)

func init() {
	fmt.Println("Init: a2")
}
```

**sample/main.go**

```
package main

import (
	"fmt"
	"sample/a"
)

func init() {
	fmt.Println("Init: main")
}
func main() {
	fmt.Println("Main Function Executing")
	a.TestA()
}
```

**Output**

```
Init: b1
Init: b2
Init: a1
Init: a1
Init: main
Main Function Executing
```

Notice in above example that init function in source files of package **b** are run first. Then init function in source files of package **a** is run and then init function in source file of main package is run. After that main function is run

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*