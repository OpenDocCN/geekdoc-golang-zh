<!--yml
category: 未分类
date: 2024-10-13 06:23:48
-->

# Select versus switch in Go (Golang)

> 来源：[https://golangbyexample.com/select-versus-switch-in-golang/](https://golangbyexample.com/select-versus-switch-in-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example of switch](#Example_of_switch "Example of switch")
*   [Example of select](#Example_of_select "Example of select")*  *# **Overview**

Below are some of the differences between **switch** and **select** statement

*   In switch each of the case statement is an expression while in select each of the case statement is either send or receive operation on a channel.

*   Format of **switch**

```
switch statement; expression {
case expression1:
     //Dosomething
case expression2:
     //Dosomething
default:
     //Dosomething
}
```

Format of **select**

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

This is how switch works. Given a **switch expression**, it goes through all cases and tries to find the first **case expression** that matches the **switch expression** otherwise the default case is executed if present. The order of matching is from top to bottom. While with a select statement ,it chooses  the case on which send or receive operation on a channel  is not blocked and is ready to be executed. If multiple cases are ready to be executed then one is choosen at random.

*   Switch statement is non-blocking.  It selects the matching case otherwise default case is executed. Even if the default block is not present and none of the case matches the switch block ends and the program proceeds. Select statement can block  as it is used with channels and channels can block on send or receive  operation. A select statement  will block if send and receive operation is blocked in all the case statements and default block is not present. Default block makes the select non-blocking as default case will be executed if all the other cases are blocked

*   A switch will go in sequence to select the matching case. So switch is deterministic. You can say lookig at switch statement ad expression, that which of the case is going to match. Select will execute a case at random with no sequence. So select is non-deterministic. It will randomly choose one of the ready case statements to execute.

*   Switch allows a fallthrough keyword to select multiple matching cases. Select doesn’t allow fallthrough keyword to select multiple cases. Only one case out of ready cases will be chosen at random.

*   Switch has two forms: expression switches and type switches while select has only one form.

Please refer to comprehensive tutorial for both

*   Switch –[https://golangbyexample.com/switch-statement-golang/](https://golangbyexample.com/switch-statement-golang/)

*   Select – [https://golangbyexample.com/select-statement-golang/](https://golangbyexample.com/switch-statement-golang/)

# **Example of switch**

```
package main

import "fmt"

func main() {
    switch ch := "b"; ch {
    case "a":
        fmt.Println("a")
    case "b":
        fmt.Println("b")    
    default:
        fmt.Println("No matching character")    
    }

    //fmt.Println(ch)

} 
```

**Output:**

```
b
```

In the above example, the switch case goes in sequence and matches the switch expression which is “b” here.

# **Example of select**

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

In the above program we created two channels which are passed to two different goroutines. Then each of the  goroutine  is sending one value to the channel. In the select  we have two case statement. Each of the two case statement is waiting for a receive operation to complete on one of the channels. Once any receive operation is complete on any of the channel it is executed and select exits. So as seen from output, in the above program it  prints the received value from one of the channel and exits.

So in the above program since it is not deterministic which of the send operation will complete earlier that is why you will see different output if you run the program different times.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*