<!--yml

类别：未分类

日期：2024-10-13 06:04:28

-->

# Go 中的模板方法设计模式（Golang）

> 来源：[`golangbyexample.com/template-method-design-pattern-golang/`](https://golangbyexample.com/template-method-design-pattern-golang/)

注意：如果有兴趣了解其他所有设计模式在 GO 中如何实现，请查看这个完整参考 – [GO 中的所有设计模式（Golang）](https://golangbyexample.com/all-design-patterns-golang/)

目录

**   介绍：

+   例子

+   完整工作代码：*  *# **介绍：**

模板方法设计模式是一种行为设计模式，它让你为特定操作定义一个模板或算法。让我们通过一个例子来理解模板设计模式。

考虑一次性密码（OTP）的例子。有不同类型的 OTP 可以触发，例如 OTP 可以是**短信** OTP 或**邮件** OTP。但无论是**短信** OTP 还是**邮件** OTP，整个 OTP 过程的步骤是相同的。步骤包括

+   生成一个随机的 n 位数字。

+   将此数字保存在缓存中以备后续验证。

+   准备内容

+   发送通知

+   发布指标

即使将来引入推送通知**OTP**，它仍然会经过上述步骤。

在这种情况下，当特定操作的步骤相同但不同实现者可以以不同方式实现这些步骤时，它就成为模板方法设计模式的候选者。我们定义一个包含固定数量方法的模板或算法。操作的实现者重写模板的方法。

现在查看下面的代码示例。

+   **iOtp**代表一个定义任何 OTP 类型应该实现的方法集的接口。

+   **短信**和**邮件**是**iOtp**接口的实现者。

+   **otp**是定义模板方法**genAndSendOTP()**的结构体，**otp**嵌入了**iOtp**接口。

重要提示：**iOtp**接口和**otp**结构的组合提供了 GO 中的抽象类的能力。参考见

> [GO 中的抽象类：完整指南](https://golangbyexample.com/go-abstract-class/)

[`golangbyexample.com/go-abstract-class/embed/#?secret=IJrupn5jVu#?secret=fqHnV3R5rZ`](https://golangbyexample.com/go-abstract-class/embed/#?secret=IJrupn5jVu#?secret=fqHnV3R5rZ)

# **例子**

**otp.go**

```go
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

```go
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

```go
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

```go
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

**输出：**

```go
SMS: generating random otp 1234
SMS: saving otp: 1234 to cache
SMS: sending sms: SMS OTP for login is 1234
SMS: publishing metrics

EMAIL: generating random otp 1234
EMAIL: saving otp: 1234 to cache
EMAIL: sending email: EMAIL OTP for login is 1234
EMAIL: publishing metrics
```

# **完整工作代码：**

```go
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

**输出：**

```go
SMS: generating random otp 1234
SMS: saving otp: 1234 to cache
SMS: sending sms: SMS OTP for login is 1234
SMS: publishing metrics

EMAIL: generating random otp 1234
EMAIL: saving otp: 1234 to cache
EMAIL: sending email: EMAIL OTP for login is 1234
EMAIL: publishing metrics
```

+   [设计](https://golangbyexample.com/tag/design/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [方法](https://golangbyexample.com/tag/method/)*   [模式](https://golangbyexample.com/tag/pattern/)*   [模板](https://golangbyexample.com/tag/template/)*
