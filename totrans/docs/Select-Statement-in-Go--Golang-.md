<!--yml
category: 未分类
date: 2024-10-13 06:23:23
-->

# Select Statement in Go (Golang)

> 来源：[https://golangbyexample.com/select-statement-golang/](https://golangbyexample.com/select-statement-golang/)

This is the  chapter 25 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [Error](https://golangbyexample.com/error-in-golang/)
**Previous Tutorial** – [Channel](https://golangbyexample.com/channel-golang/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Use of  select statement](#Use_of_select_statement "Use of  select statement")
*   [Select with send operation example](#Select_with_send_operation_example "Select with send operation example")
*   [Select with default case](#Select_with_default_case "Select with default case")
*   [Select with blocking timeout](#Select_with_blocking_timeout "Select with blocking timeout")
*   [Empty select](#Empty_select "Empty select")
*   [Select statement with an infinite for loop outside](#Select_statement_with_an_infinite_for_loop_outside "Select statement with an infinite for loop outside")
*   [Select statement with a nil channel](#Select_statement_with_a_nil_channel "Select statement with a nil channel")
*   [Break keyword in Select](#Break_keyword_in_Select "Break keyword in Select")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

Select is similar to switch statement, the difference being that in select each of the case statement waits for a send or receive operation from a channel. Select statement will wait until send or receive operation is completed on any one of the case statements. It is different from the switch statement in the way that each of the case statements will either send or receive operation on a channel whereas in switch each of the case statements is an expression.  So a select statement lets you wait on multiple send and receive operations from different channels.  Two important points to note about set a statement is

*   The select blocks until any of the case statements are ready. 
*   If multiple case statements are ready then it selects one at random and proceeds. 

Below is the format of **select**

```
select {
case channel_send_or_receive:
     //Dosomething
case channel_send_or_receive:
     //Dosomething
default:
     //Dosomething
}
```

Select chooses  the case on which send or receive operation on a channel is not blocked and is ready to be executed. If multiple cases are ready to be executed then one is choosen at random.

Let’s see a simple example. We will study about default case later in this tutorial.

```
package main

import "fmt"

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)

    go goOne(ch1)
    go goTwo(ch2)

    select {
    case msg1 := <-ch1:
        fmt.Println(msg1)
    case msg2 := <-ch2:
        fmt.Println(msg2)
    }
}

func goOne(ch chan string) {
    ch <- "From goOne goroutine"
}

func goTwo(ch chan string) {
    ch <- "From goTwo goroutine"
}
```

**Output**

```
From goOne goroutine
```

In the above program, we created two channels that are passed to two different goroutines. Then each of the goroutines is sending one value to the channel. In the select, we have two case statements. Each of the two **case** statements is waiting for a receive operation to complete on one of the channels. Once any receive operation is complete on any of the channels it is executed and **select** exits. So as seen from the output, in the above program, it prints the received value from one of the channels and exits.

So in the above program since it is not deterministic which of the send operation will complete earlier that is why you will see different outputs if you run the program multiple times. Let's see another program where we will put timeout in the goroutine goTwo before sending the value to the **ch2** channel. This will make sure that the send operation on **ch1** will be executed earlier than send operation to **ch2**.

```
package main

import (
    "fmt"
    "time"
)

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)

    go goOne(ch1)
    go goTwo(ch2)

    select {
    case msg1 := <-ch1:
        fmt.Println(msg1)
    case msg2 := <-ch2:
        fmt.Println(msg2)
    }
}

func goOne(ch chan string) {
    ch <- "From goOne goroutine"
}

func goTwo(ch chan string) {
    time.Sleep(time.Second * 1)
    ch <- "From goTwo goroutine"
}
```

**Output**

```
From goOne goroutine
```

In the above program, in select statement the receive operation on **ch1** is completed earlier and hence select will always be executing that case statement as also evident from the output

It is also possible to wait for receive operation to complete on both the channels by using a for loop across the select statement. Let's see a program for that

```
package main

import "fmt"

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)
    go goOne(ch1)
    go goTwo(ch2)
    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-ch1:
            fmt.Println(msg1)
        case msg2 := <-ch2:
            fmt.Println(msg2)
        }
    }
}

func goOne(ch chan string) {
    ch <- "From goOne goroutine"
}

func goTwo(ch chan string) {
    ch <- "From goTwo goroutine"
}
```

**Output**

```
From goOne goroutine
From goTwo goroutine
```

In the above program, we put a for loop of length two across the select statement. Hence the select statement is executed twice and prints the received value from each of the case statements.

We mentioned earlier that select can block if any of the case statement is not ready. Let's see an example for that

```
package main

import "fmt"

func main() {
    ch1 := make(chan string)
    select {
    case msg := <-ch1:
        fmt.Println(msg)
    }
}
```

**Output**

```
fatal error: all goroutines are asleep - deadlock!
```

In the above program, we created a channel named **ch1**.  We then receive from this channel in the select statement.  Since no goroutine  is sending to this channel hence it results in a deadlock and select statement blocks indefinitely.  That is why it gives  below output

```
fatal error: all goroutines are asleep - deadlock!
```

# **Use of  select statement**

The select statement is useful if there are multiple goroutines that are sending data to multiple channels concurrently. The select statement can then receive the data concurrently from any of one goroutine and execute the statement which is ready. So select along with channels and goroutines become a very powerful tool for managing synchronization and concurrency.

# **Select with send operation example**

So far we have seen examples of receive operation in select case statements. Let's see an example of send operation as well

```
package main

import "fmt"

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)
    go goOne(ch1)
    go goTwo(ch2)
    select {

    case msg1 := <-ch1:
        fmt.Println(msg1)
    case ch2 <- "To goTwo goroutine":
    }
}

func goOne(ch chan string) {
    ch <- "From goOne goroutine"
}

func goTwo(ch chan string) {
    msg := <-ch
    fmt.Println(msg)
}
```

**Output**

```
To goTwo goroutine
```

# **Select with default case**

Similar to switch select can also have a default case.  This default case will be executed if no send it or receive operation is ready on any of the case statements. So in a way default statement prevents the select from blocking forever.  So a very important point to note is that the default statement makes the select non-blocking. If the select statement doesn't contain a default case then it can block forever until one send or receive operation is ready on any case statement. Let's see an example to fully understand it

```
package main

import "fmt"

func main() {
    ch1 := make(chan string)
    select {
    case msg := <-ch1:
        fmt.Println(msg)
    default:
        fmt.Println("Default statement executed")
    }
}
```

**Output**

```
Default statement executed
```

In the above program have a select statement which is waiting for receive operation on **ch1** and a default statement. Since no goroutine is sending to channel **ch1**, hence the default case is executed and select exits. If default case were not there the select would have blocked.

# **Select with blocking timeout**

Blocking timeout in select can be achieved by using **After()** function of **time** package. Below  is the signature of **After()** function.

```
func After(d Duration) <-chan Time
```

The **After** function waits for d duration  to finish and then it returns the current time on a channel

[https://golang.org/pkg/time/#Time.After](https://golang.org/pkg/time/#Time.After)

Let's see a program of select with timeout

```
package main

import (
	"fmt"
	"time"
)

func main() {
	ch1 := make(chan string)
	go goOne(ch1)

	select {
	case msg := <-ch1:
		fmt.Println(msg)
	case <-time.After(time.Second * 1):
		fmt.Println("Timeout")
	}
}

func goOne(ch chan string) {
	time.Sleep(time.Second * 2)
	ch <- "From goOne goroutine"
}
```

**Output**

```
Timeout
```

In the above select statement we are waiting for receive operation to complete on **ch1**. In other case statement we have **time.After** with the duration of 1 second. So essentially this select statement will wait for at least 1 second for receive operation to complete on **ch1** and after that the **time.After** case statement will be executed.  We have put a timeout of more than 1 seconds in the **goOne** function and hence we  see the **time.After** statement getting executed and

```
Timeout
```

getting printed as output

# **Empty select**

Select block without any case statement is empty select. The empty select will block forever as there is no case statement  to execute. It is also one of the way for a goroutine to wait indefinitely. But if this empty select is put in the main goroutine then it will cause a deadlock. Let's see a program

```
package main

func main() {
    select {}
}
```

**Output**

```
fatal error: all goroutines are asleep - deadlock!
```

In the above program we have empty select and hence it results in a deadlock that is why you see the output as below

```
fatal error: all goroutines are asleep - deadlock!
```

# **Select statement with an infinite for loop outside**

We can have infinite for loop outside a select statement. This will cause the select statement to execute indefinite number of times.So when using for statement with infinite loop outside the select statement, we need to have a way yo break out of the for loop. One of the use case of having infinite for loop outside select statement could be that you are waiting for multiple operations to receive on the same channel for a certain time. See below example

```
package main

import (
	"fmt"
	"time"
)

func main() {
	news := make(chan string)
	go newsFeed(news)

	printAllNews(news)
}

func printAllNews(news chan string) {
	for {
		select {
		case n := <-news:
			fmt.Println(n)
		case <-time.After(time.Second * 1):
			fmt.Println("Timeout: News feed finished")
			return
		}
	}
}

func newsFeed(ch chan string) {
	for i := 0; i < 2; i++ {
		time.Sleep(time.Millisecond * 400)
		ch <- fmt.Sprintf("News: %d", i+1)
	}
}
```

**Output**

```
News: 1
News: 2
Timeout: News feed finished
```

In the above program, we have created a channel named **news**  which will hold data of string type. Then we pass this channel to the **newsfeed** function which is pushing the news feed to this channel . In the select statement, we are receiving the news feed from the **news** channel. This select statement is inside an infinite for loop  so the select statement will be executed multiple times until we  exit out of for loop . We also have **time.After** with a duration for 1 second as one of the case statements. So this set up will receive all the news from the **news** channel for  1  second and then exit. 

# **Select statement with a nil channel**

Send or receive operation on nil channel blocks forever. Hence a use case of having a nil channel in the select statement is to disable that case statement after the the send or receive operation is completed on that case statement. The channel then can simply be set to nil. That case statement will be  ignored when the select statement is executed again and receive or send operation will be waited on another case statement. So it is meant to ignore that case statement and execute the other case statement

```
package main

import (
    "fmt"
    "time"
)

func main() {
    news := make(chan string)
    go newsFeed(news)
    printAllNews(news)
}

func printAllNews(news chan string) {
    for {
        select {
        case n := <-news:
            fmt.Println(n)
            news = nil
        case <-time.After(time.Second * 1):
            fmt.Println("Timeout: News feed finished")
            return
        }
    }
}

func newsFeed(ch chan string) {
    for i := 0; i < 2; i++ {
        time.Sleep(time.Millisecond * 400)
        ch <- fmt.Sprintf("News: %d", i+1)
    }
}
```

**Output**

```
News: 1
Timeout: News feed finished
```

The above program is pretty much similar to the program we studied above related to having a select statement inside infinite for loop. The only change being that  after we receive the first news, we disabled the case statement by setting the news channel to nil.

```
case n := <-news:
   fmt.Println(n)
   news = nil
```

Hence  we only received the first news and after that, it times out.  This is the use case of nil channel in the select statement

# **Break keyword in Select**

Below is the **break** keyword example.

```
import "fmt"

func main() {
	ch := make(chan string, 1)
	ch <- "Before break"

	select {
	case msg := <-ch:
		fmt.Println(msg)
		break
		fmt.Println("After break")
	default:
		fmt.Println("Default case")
	}
}
```

**Output**

```
Before break
```

**break** statement will terminate the execution of innermost statement and below line below will never be executed

```
fmt.Println("After break")
```

# **Conclusion**

This is all about the select statement in golang. Hope you have liked this article. Please share feedback/improvements/mistakes in comments

**Next Tutorial** – [Error](https://golangbyexample.com/error-in-golang/)
**Previous Tutorial** – [Channel](https://golangbyexample.com/channel-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*