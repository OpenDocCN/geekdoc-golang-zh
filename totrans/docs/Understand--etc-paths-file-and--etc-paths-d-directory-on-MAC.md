<!--yml
category: 未分类
date: 2024-10-13 06:11:46
-->

# Understand /etc/paths file and /etc/paths.d directory on MAC

> 来源：[https://golangbyexample.com/understand-etc-paths-pathsd-mac/](https://golangbyexample.com/understand-etc-paths-pathsd-mac/)

MAC OS uses **/etc/paths** file and **/etc/paths.d** directory for setting up the value of **PATH** env varialb.e.  A utility **path_helper** is used to set the **PATH** env variable based on the content of **/etc/paths** file and content of files in **/etc/paths.d** directory. This is how you can run the path_helper

```
eval `/usr/libexec/path_helper -s`
```

Let’s see how these two play role in setting up the path

**/etc/paths file**

This file contains the paths that need to be set in **PATH** env variable. If I print the contents of this file on my MAC, it outputs

```
/ $ cat /etc/paths
/usr/local/bin
/usr/bin
/bin
/usr/sbin
/sbin
```

All the above paths are added to the PATH variable.

**/etc/paths.d directory**

This directory contains the list of files. Each of the files contains the path that needs to be added to the PATH variable. As a demonstration, If I install GO on my MAC using the .pkg installer for GO available here – [https://golang.org/dl/](https://golang.org/dl/).

Then after installation, it will create a  file named  **‘go’** in the **‘/etc/paths.d’** directory. Let’s output the contents of the file.

```
/ $ cat /etc/paths.d/go
/usr/local/go/bin
```

It outputs ‘/usr/local/go/bin’ and this path will be added to the PATH env variable.

**Points to Note:**

The primary reason that **‘/etc/paths.d’** directory is used because **‘/etc/paths’** will be modified and/or replaced by system upgrades while contents of **‘/etc/paths.d’** directory will never be affected by such changes.