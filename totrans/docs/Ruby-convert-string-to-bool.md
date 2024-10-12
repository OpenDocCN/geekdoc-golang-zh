<!--yml
category: 未分类
date: 2024-10-13 06:52:56
-->

# Ruby convert string to bool

> 来源：[https://golangbyexample.com/string-bool-ruby/](https://golangbyexample.com/string-bool-ruby/)

Table of Contents

 **   [Overview](#Overview "Overview")*  *# **Overview**

In the Ruby language, strings “true” and “false” are interpreted as true when used in the **if** condition. See example below

```
if "false"
   puts "yes"
end
```

**Output**

```
"yes"
```

“false” evaluates to true

Therefore it becomes important to handle string “false” or string “true” correctly.

We can create a custom method that can return boolean true or false based on the content of the string

```
def true?(str)
  str.to_s.downcase == "true"
end
```

We can try out the above function

```
true?("false")
 => false
```

```
true?("true")
 => true
```

Also, note that for strings other than “false” it will give true in return, but the function could be easily modified to handle that case*