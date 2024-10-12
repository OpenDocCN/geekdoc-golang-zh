<!--yml
category: 未分类
date: 2024-10-13 06:03:30
-->

# Singleton Design Pattern in Go (Golang)

> 来源：[https://golangbyexample.com/singleton-design-pattern-go/](https://golangbyexample.com/singleton-design-pattern-go/)

Note: Interested in understanding how all other design patterns can be implemented in GO. Please see this full reference – [All Design Patterns in Go (Golang)](https://golangbyexample.com/all-design-patterns-golang/)

**Introduction:**

Singleton Design Pattern is a creational design pattern and also one of the most commonly used design pattern. This pattern is used when only a single instance of the struct should exist. This single instance is called a singleton object. Some of the cases where the singleton object is applicable:

1.  **DB instance** – we only want to create only one instance of DB object and that instance will be used throughout the application. 
2.  **Logger instance** – again only one instance of the logger should be created and it should be used throughout the application.

The singleton instance is created when the struct is first initialized.  Usually, there is getInstance() method defined on the struct for which only one instance needs to be created. Once created then the same singleton instance is returned every time by the **getInstance()**.

In GO we have goroutines. Hence the singleton struct should return the same instance whenever multiple goroutines are trying to access that instance. It is very easy to get a singleton design pattern wrong. The below code illustrates the right way to create a singleton object. 

```
var lock = &sync.Mutex{}

type single struct {
}

var singleInstance *single

func getInstance() *single {
    if singleInstance == nil {
        lock.Lock()
        defer lock.Unlock()
        if singleInstance == nil {
            fmt.Println("Creting Single Instance Now")
            singleInstance = &single{}
        } else {
            fmt.Println("Single Instance already created-1")
        }
    } else {
        fmt.Println("Single Instance already created-2")
    }
    return singleInstance
}
```

Above code ensures that only one instance of the single struct is created. Some point worth noting. 

1.  There is a check at the start for nil **singleInstance**. This is to prevent the expensive lock operations every time **getinstance()** method is called. If this check fails then it means that **singleInstance** is already created
2.  The **singleInstance** is created inside the lock.
3.  There is another check for nil **singleIinstance** after the lock is acquired. This is to make sure that if more than one goroutine bypass the first check then only one goroutine is able to create the singleton instance otherwise each of the goroutine will create its own instance of the **single** struct.

Here is the full code

**single.go**

```
package main

import (
    "fmt"
    "sync"
)

var lock = &sync.Mutex{}

type single struct {
}

var singleInstance *single

func getInstance() *single {
    if singleInstance == nil {
        lock.Lock()
        defer lock.Unlock()
        if singleInstance == nil {
            fmt.Println("Creting Single Instance Now")
            singleInstance = &single{}
        } else {
            fmt.Println("Single Instance already created-1")
        }
    } else {
        fmt.Println("Single Instance already created-2")
    }
    return singleInstance
}
```

**main.go**

```
package main

import (
    "fmt"
)

func main() {
    for i := 0; i < 100; i++ {
        go getInstance()
    }
    // Scanln is similar to Scan, but stops scanning at a newline and
    // after the final item there must be a newline or EOF.
    fmt.Scanln()
}
```

**Output:**

```
Creting Single Instance Now
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-2
Single Instance already created-2
Single Instance already created-2
Single Instance already created-2
Single Instance already created-2
Single Instance already created-2
Single Instance already created-2
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-2
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
```

**Note:**

*   There is one output of **"Creating Single Instance Now"** meaning only one goroutine was able to create a single instance.
*   There are a couple of outputs of **"Single Instance already created-1"** meaning that some of the goroutines found the value of singleInstance as nil in the first check and bypassed that.
*   There are couple of output of **"Single Instance already created-2"** meaning by the time they reached the single instance was already created and they could not bypass the first if check

**Other methods of creating a singleton object in Go**

*   **init() function**

We can create a single instance inside the init function. This is only applicable if the early initialization of the object is ok. The init function is only called once per file in a package,  so we can be sure that only a single instance will be created.

*   **sync.Once**

The sync.Once will only perform the operation only once. See below code

**single.go**

```
var once sync.Once

type single struct {
}

var singleInstance *single

func getInstance() *single {
    if singleInstance == nil {
        once.Do(
            func() {
                fmt.Println("Creting Single Instance Now")
                singleInstance = &single{}
            })
    } else {
        fmt.Println("Single Instance already created-2")
    }
    return singleInstance
}
```

**Output:**

```
Creting Single Instance Now
Single Instance already created-2
Single Instance already created-2
```

*   There is one output of **"Creating Single Instance Now"** meaning only one goroutine was able to create the single instance
*   There are couple of output of **"Single Instance already created-2"** meaning by the time they reached the single instance was already created and they could not bypass the first if check

*   [singleton](https://golangbyexample.com/tag/singleton/)*   [singleton design pattern in golang](https://golangbyexample.com/tag/singleton-design-pattern-in-golang/)