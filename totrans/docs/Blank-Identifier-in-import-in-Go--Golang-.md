<!--yml
category: 未分类
date: 2024-10-13 06:30:44
-->

# Blank Identifier in import in Go (Golang)

> 来源：[https://golangbyexample.com/blank-identifier-import-golang/](https://golangbyexample.com/blank-identifier-import-golang/)

Blank identifier in importing packages means specifying a blank import for the imported package. The syntax for it is

```
import _ <directory_path></directory_path>
```

What is this blank import and why it is used. For this you have to understand two things

1.  About **init** function
2.  About blank identifier represented by an underscore (‘**_**‘)

**init() function**

init() function is a special function that is used to initialize the global variables of a package. These functions are executed when the package is initialized. Each of the GO source files in a package can have its own init() function. Whenever you import any package in the program, then on the execution of that program, init functions(if present)  in the GO source files belonging to that imported package are called first. Some points to note about init function

*   Init function is optional

*   Init function does not take any argument

*   Init function does not have any return value.

*   Init function is called implicitly. Since it is called implicitly, init function cannot reference it from anywhere.

*   There can be multiple init() functions within the same source file.

**About blank identifier**

go doesn’t allow any unused variable. Any unused variable can be replaced by a blank identifier (‘_’) .
So now a blank import of a package is used when

*   Imported package is not being used in the current program

*   But we intend to import that package so that init function in the GO source files belonging to that package can be called and initiazation of variables in that package can be done properly

So basically a blank import is used when a package is solely imported for its side effects.

As an example mysql package is used as blank import for its side-effect of registering the mysql driver as a database driver in the init()  function of mysql package, without importing any other functions:

```
_ "github.com/go-sql-driver/mysql"
```

For using any of the mysql library such as gorm or xorm above blank import is necessary

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)