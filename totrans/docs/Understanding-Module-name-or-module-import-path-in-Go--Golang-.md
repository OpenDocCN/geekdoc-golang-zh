<!--yml
category: 未分类
date: 2024-10-13 06:29:54
-->

# Understanding Module name or module import path in Go (Golang)

> 来源：[https://golangbyexample.com/module-import-path-golang/](https://golangbyexample.com/module-import-path-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [The module is a utility module and you plan to publish your module to VCS like git](#The_module_is_a_utility_module_and_you_plan_to_publish_your_module_to_VCS_like_git "The module is a utility module and you plan to publish your module to VCS like git")
*   [The module is a utility module and you don’t plan to publish your module](#The_module_is_a_utility_module_and_you_dont_plan_to_publish_your_module "The module is a utility module and you don’t plan to publish your module")
*   [The module is a executable module](#The_module_is_a_executable_module "The module is a executable module")*  *# **Overview**

Module is go support for dependency management. A module by definition is a collection of related packages with **go.mod** at its root.  The **go.mod** file defines the

*   Module import path.

*   Dependency requirements of the module for a successful build. It defines both project’s dependencies requirement and also locks them to their correct version.

Now let’s discuss more in detail about the module import path. It is the prefix path that is used to import all packages of that module. Let’s see an example to better understand the module import path. For that let’s create a module

```
go mod init sample.com/learn
```

Above command will create a module with import path as [sample.com](http://sample.com)/learn. It will also create a **go.mod** file in the same directory. Let’s examine the contents of this file. Do a cat **go.mod**

```
module sample.com/learn

go 1.14
```

The first line of the go.mod file will have the module import path as you can see in the **go.mod** file above as well. Now where is this module import path useful. The import path of a module is used to import any package of that module from any other module. This any other module will use module import path as prefix path plus the folder path of the package.

```
module_import_path + folder_path_of_package
```

So if the **sample.com/learn** module contains a **math** package present at directory **math**, then any other module can import the **math** package using the path

```
sample.com/learn/math
```

Now let’s see what factors decide the naming of the module import path. There can be three cases that decide what import pathname can be used with modules.

*   The module is a utility module and you plan to publish your module

*   The module is a utility module and you don’t plan to publish your module

*   The module is an executable module

# **The module is a utility module and you plan to publish your module to VCS like git**

If you plan to publish your module then the module name should match the URL of the repo that host that module. Go tries to download dependencies from the VCS using the import path of the module.

For example, notice the module import path of this module [https://github.com/pborman/uuid](https://github.com/pborman/uuid) . The go.mod file is as below

```
module github.com/pborman/uuid

...
```

Notice that the URL of the module is same as module import path.

# **The module is a utility module and you don’t plan to publish your module**

This is the case when you only mean to use the utility module locally only. In this case, the import path can be anything. Refer to this example where we illustrated how we can import a local module. In the example, we have used a name for the module import path which doesn’t represent the VCS repository.

[https://golangbyexample.com/import-local-module-golang](https://golangbyexample.com/import-local-module-golang)/

# **The module is a executable module**

In this case, also the module import path can be anything. The module import path can be a non-URL even if you plan to commit your module into VCS as it will not be used by any other module.

However, it is a good practice to use a meaningful import path while creating a module

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*