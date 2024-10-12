<!--yml
category: 未分类
date: 2024-10-13 06:29:23
-->

# Selection of the version of library or dependency in Go (Golang)

> 来源：[https://golangbyexample.com/versiono-module-selection-go/](https://golangbyexample.com/versiono-module-selection-go/)

To understand how does GO’s approach while selecting the version of the library of which two versions are specified in the **go.mod** file, we have to first understand Semantic Versioning

Semantic Versioning is comprised of three parts separated by dots. Below is the format for versioning.

```
v{major_version}.{minor_version}.{patch_version}
```

where

*   **v** – it is just an indicator that it is a version

*   **major_version** – It represents the incompatible API changes in the library. So when there are changes in the library that is not backward compatible, in that case, major_version is incremented
*   **minor_version** – It represents the change in functionality of the library in a backward-compatible manner. So when there are some functionality changes in the library but those changes are backward compatible then, in that case, the minor version is incremented

*   **patch_version** – It represents the bug fixes in the library in a backward-compatible manner. So when there are bug fixes to the existing functionality of the library, then, in that case, patch_version is incremented.

Now there can be two cases while selecting the version of the library

*   Two versions of the same library are used which only differ in the minor and patch version. Their major version is the same.

*   Two versions of the same library are used which differ in the major.

Let’s see what approach does go follows in the above two cases

Table of Contents

 **   [Differ in minor or patch version](#Differ_in_minor_or_patch_version "Differ in minor or patch version")
*   [Differ in major version](#Differ_in_major_version "Differ in major version")*  *# **Differ in minor or patch version**

Go follows the minimum version policy approach while selecting the version of the library of which two versions are specified in the **go.mod** file which differ only in their minor or patch version.

For example in case you are using the two versions of same library which are

```
1.2.0
```

and

```
1.3.0
```

then go will choose 1.3.0 as it is the latest version.

# **Differ in major version**

Go treats the major version as a different module itself. Now, what does that means? This essentially means that the import path will have a major version as its suffix. Let’s take the example of any go library. Let’s latest semantic version is

```
v8.2.3
```

Then the go.mod file will like below

```
module github.com/sample/v8
go 1.11

..
```

It has major version in its import path. So any library which is using the go-redis have to import it like

```
import "github.com/sample/v8"
```

If in future **v9** version is released than it has to be imported in the application like

```
import "github.com/sample/v9"
```

Also the library will change its go.mod file to reflect the v9 major version

```
module github.com/sample/v9
```

What it essentially allows is to use different major version of the same library to be used within same go application.  We can also give meaningful names when different major version of the same library is imported in the same application. For eg

```
import sample_v8 "github.com/sample/v8"
import sample_v9 "github.com/sample/v9"
```

This is also known as **Semantic Import Versioning**

Also note that

*   For the first version, it is ok to not specify the version in the go.mod file.

*   Also, be careful when importing a different major version of the same library. Look out for the new functionality that might be available with new versions.

Also for the same reason when you update a specific module using

```
go get -u
```

then it will only upgrade to the latest minor version or patch version whichever applicable. For example let’s say the current version used by an application is

```
v1.1.3
```

Also let’s say we have below versions available

```
v1.2.0
v2.1.0
```

Then when we run

```
go get
```

then it will update to

```
v1.2.0
```

The reason is because go get will only update the minor or patch version but never the major version as go treats the major version of a module as a different module entirely.

To upgrade the major version, specify that  upgraded dependency explicitly  in the **go.mod** file or do a go get of that version.

Also, a couple of points to note about upgrading module

*   To upgrade a dependency to its latest patch version only, use the below command

```
go get -u=patch <dependency_name></dependency_name>
```

*   To upgrade a dependency to a specific version, use the below command

```
go get dependency@version
```

*   To upgrade a dependency to a specific commit, use the below command

```
go get <dependency_name>@commit_number</dependency_name>
```

*   To upgrade all dependency to their latest minor and patch version, use the below command

```
go get ./...
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*