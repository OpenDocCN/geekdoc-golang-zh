<!--yml
category: 未分类
date: 2024-10-13 06:41:18
-->

# Load a .env or environment file in Go (Golang)

> 来源：[https://golangbyexample.com/load-env-fiie-golang/](https://golangbyexample.com/load-env-fiie-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

**gotenv** package in golang can be used to load **.env** or **environment** file in golang
[https://github.com/joho/godotenv](https://github.com/joho/godotenv)

Below is the signature of the function

```
func Load(filenames ...string) (err error) 
```

It takes a variable number of arguments where each argument can be the **.env** filenames that it needs to load

# **Program**

Create a file **local.env** with below contents

```
STACK=DEV
DATABASE=SQL
```

Here is the program

```
package main

import (
	"fmt"
	"log"
	"os"

	"github.com/joho/godotenv"
)

func main() {
	err := godotenv.Load("local.env")
	if err != nil {
		log.Fatalf("Some error occured. Err: %s", err)
	}

	val := os.Getenv("STACK")
	fmt.Println(val)

	val = os.Getenv("DATABASE")
	fmt.Println(val)
}
```

**Output**

```
DEV
SQL
```

It loads the **local.env** file and gives the correct output

It can also be used to load multiple .env files. Create a new file **test.env** with below contents

```
TEST=UNIT
```

```
package main

import (
	"fmt"
	"log"
	"os"

	"github.com/joho/godotenv"
)

func main() {
	err := godotenv.Load("local.env", "test.env")
	if err != nil {
		log.Fatalf("Some error occured. Err: %s", err)
	}

	val := os.Getenv("STACK")
	fmt.Println(val)

	val = os.Getenv("DATABASE")
	fmt.Println(val)

	val = os.Getenv("TEST")
	fmt.Println(val)
}
```

**Output**

```
DEV
SQL
UNIT
```

If you don’t supply any argument to the **Load** function then by default it will load the file named **.env** in the current directory.

Create a file named .**env** with the below contents in the current directory

```
STACK=DEV
DATABASE=SQL
```

```
package main

import (
	"fmt"
	"log"
	"os"

	"github.com/joho/godotenv"
)

func main() {
	err := godotenv.Load()
	if err != nil {
		log.Fatalf("Some error occured. Err: %s", err)
	}

	val := os.Getenv("STACK")
	fmt.Println(val)

	val = os.Getenv("DATABASE")
	fmt.Println(val)
}
```

Notice in the above program that we didn’t pass any argument to the **Load** function

**Output**

```
DEV
SQL
```

This was about loading **.env** files in golang

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*