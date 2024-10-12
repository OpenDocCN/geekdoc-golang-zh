<!--yml
category: 未分类
date: 2024-10-13 06:04:28
-->

# Template Method Design Pattern in Go (Golang)

> 来源：[https://golangbyexample.com/template-method-design-pattern-golang/](https://golangbyexample.com/template-method-design-pattern-golang/)

Note: Interested in understanding how all other design patterns can be implemented in GO. Please see this full reference – [All Design Patterns in Go (Golang)](https://golangbyexample.com/all-design-patterns-golang/)

Table of Contents

 **   [Introduction:](#Introduction "Introduction:")
*   [Example](#Example "Example")
*   [Full Working Code:](#Full_Working_Code "Full Working Code:")*  *# **Introduction:**

Template Method Design Pattern is a behavioral design pattern that lets you define a template or algorithm for a particular operation.  Let’s understand the template design pattern with an example.

Consider the example of One Time Password or OTP. There are different types of OTP that can be triggered for eg. OTP can be **SMS** OTP or **EMAIL** OTP.  But irrespective it is an **SMS** OTP or **EMAIL** OTP,  the entire steps of the OTP process are the same.  The steps are

*   Generate a random n digit number. 

*   Save this number in the cache for later verification. 

*   Prepare the content

*   Send the notification

*   Publish the metrics

Even in the future let’s say a push notification **OTP** is introduced but still it will go through the above steps.

In such scenarios when the steps of a particular operation are the same but the steps of the operations can be implemented in a different way by different implementors , then that becomes a candidate for the Template Method Design Pattern. We define a template or algorithm which comprises of a fixed number of methods. The implementer of the operation overrides the methods of the template.

Now check out the below code example.

*   **iOtp** represents an interface that defines the set of methods that any otp type should implement

*   **sms** and **email** are the implementors of **iOtp** interface

*   **otp** is the struct which defines the template method **genAndSendOTP(). otp** embeds **iOtp** interface. 

Important: The combination of iOtp interface and otp struct provides the capabilities of Abstract Class in go. For reference see

> [Abstract Class in GO: Complete Guide](https://golangbyexample.com/go-abstract-class/)

[https://golangbyexample.com/go-abstract-class/embed/#?secret=IJrupn5jVu#?secret=fqHnV3R5rZ](https://golangbyexample.com/go-abstract-class/embed/#?secret=IJrupn5jVu#?secret=fqHnV3R5rZ)

# **Example**

**otp.go**

```
package main

type iOtp interface {
    genRandomOTP(int) string
    saveOTPCache(string)
    getMessage(string) string
    sendNotification(string) error
    publishMetric()
}

type otp struct {
    iOtp iOtp
}

func (o *otp) genAndSendOTP(otpLength int) error {
    otp := o.iOtp.genRandomOTP(otpLength)
    o.iOtp.saveOTPCache(otp)
    message := o.iOtp.getMessage(otp)
    err := o.iOtp.sendNotification(message)
    if err != nil {
        return err
    }
    o.iOtp.publishMetric()
    return nil
}
```

**sms.go**

```
package main

import "fmt"

type sms struct {
    otp
}

func (s *sms) genRandomOTP(len int) string {
    randomOTP := "1234"
    fmt.Printf("SMS: generating random otp %s\n", randomOTP)
    return randomOTP
}

func (s *sms) saveOTPCache(otp string) {
    fmt.Printf("SMS: saving otp: %s to cache\n", otp)
}

func (s *sms) getMessage(otp string) string {
    return "SMS OTP for login is " + otp
}

func (s *sms) sendNotification(message string) error {
    fmt.Printf("SMS: sending sms: %s\n", message)
    return nil
}

func (s *sms) publishMetric() {
    fmt.Printf("SMS: publishing metrics\n")
}
```

**email.go**

```
package main

import "fmt"

type email struct {
    otp
}

func (s *email) genRandomOTP(len int) string {
    randomOTP := "1234"
    fmt.Printf("EMAIL: generating random otp %s\n", randomOTP)
    return randomOTP
}

func (s *email) saveOTPCache(otp string) {
    fmt.Printf("EMAIL: saving otp: %s to cache\n", otp)
}

func (s *email) getMessage(otp string) string {
    return "EMAIL OTP for login is " + otp
}

func (s *email) sendNotification(message string) error {
    fmt.Printf("EMAIL: sending email: %s\n", message)
    return nil
}

func (s *email) publishMetric() {
    fmt.Printf("EMAIL: publishing metrics\n")
}
```

**main.go**

```
package main

import "fmt"

func main() {
    smsOTP := &sms{}
    o := otp{
        iOtp: smsOTP,
    }
    o.genAndSendOTP(4)
    fmt.Println("")
    emailOTP := &email{}
    o = otp{
        iOtp: emailOTP,
    }
    o.genAndSendOTP(4)
}
```

**Output:**

```
SMS: generating random otp 1234
SMS: saving otp: 1234 to cache
SMS: sending sms: SMS OTP for login is 1234
SMS: publishing metrics

EMAIL: generating random otp 1234
EMAIL: saving otp: 1234 to cache
EMAIL: sending email: EMAIL OTP for login is 1234
EMAIL: publishing metrics
```

# **Full Working Code:**

```
package main

import "fmt"

type iOtp interface {
    genRandomOTP(int) string
    saveOTPCache(string)
    getMessage(string) string
    sendNotification(string) error
    publishMetric()
}

type otp struct {
    iOtp iOtp
}

func (o *otp) genAndSendOTP(otpLength int) error {
    otp := o.iOtp.genRandomOTP(otpLength)
    o.iOtp.saveOTPCache(otp)
    message := o.iOtp.getMessage(otp)
    err := o.iOtp.sendNotification(message)
    if err != nil {
        return err
    }
    o.iOtp.publishMetric()
    return nil
}

type sms struct {
    otp
}

func (s *sms) genRandomOTP(len int) string {
    randomOTP := "1234"
    fmt.Printf("SMS: generating random otp %s\n", randomOTP)
    return randomOTP
}

func (s *sms) saveOTPCache(otp string) {
    fmt.Printf("SMS: saving otp: %s to cache\n", otp)
}

func (s *sms) getMessage(otp string) string {
    return "SMS OTP for login is " + otp
}

func (s *sms) sendNotification(message string) error {
    fmt.Printf("SMS: sending sms: %s\n", message)
    return nil
}

func (s *sms) publishMetric() {
    fmt.Printf("SMS: publishing metrics\n")
}

type email struct {
    otp
}

func (s *email) genRandomOTP(len int) string {
    randomOTP := "1234"
    fmt.Printf("EMAIL: generating random otp %s\n", randomOTP)
    return randomOTP
}

func (s *email) saveOTPCache(otp string) {
    fmt.Printf("EMAIL: saving otp: %s to cache\n", otp)
}

func (s *email) getMessage(otp string) string {
    return "EMAIL OTP for login is " + otp
}

func (s *email) sendNotification(message string) error {
    fmt.Printf("EMAIL: sending email: %s\n", message)
    return nil
}

func (s *email) publishMetric() {
    fmt.Printf("EMAIL: publishing metrics\n")
}

func main() {
    smsOTP := &sms{}
    o := otp{
        iOtp: smsOTP,
    }
    o.genAndSendOTP(4)
    fmt.Println("")
    emailOTP := &email{}
    o = otp{
        iOtp: emailOTP,
    }
    o.genAndSendOTP(4)
}
```

**Output:**

```
SMS: generating random otp 1234
SMS: saving otp: 1234 to cache
SMS: sending sms: SMS OTP for login is 1234
SMS: publishing metrics

EMAIL: generating random otp 1234
EMAIL: saving otp: 1234 to cache
EMAIL: sending email: EMAIL OTP for login is 1234
EMAIL: publishing metrics
```

*   [design](https://golangbyexample.com/tag/design/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [method](https://golangbyexample.com/tag/method/)*   [pattern](https://golangbyexample.com/tag/pattern/)*   [template](https://golangbyexample.com/tag/template/)*