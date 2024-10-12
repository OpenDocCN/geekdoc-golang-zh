<!--yml
category: 未分类
date: 2024-10-13 06:31:18
-->

# GO Installation

> 来源：[https://golangbyexample.com/golang-installation/](https://golangbyexample.com/golang-installation/)

This is the chapter 2 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [Set up GO Workspace and Hello World Program](https://golangbyexample.com/workspace-hello-world-golang/)
**Previous Tutorial** – [About Golang](https://golangbyexample.com/about-golang/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Installation on MAC](#Installation_on_MAC "Installation on MAC")
    *   [Using .pkg installer](#Using_pkg_installer "Using .pkg installer")
    *   [Using archive](#Using_archive "Using archive")
    *   [Using brew](#Using_brew "Using brew")
*   [Installation on Linux and Free BSD](#Installation_on_Linux_and_Free_BSD "Installation on Linux and Free BSD")
    *   [Downloading the archive](#Downloading_the_archive "Downloading the archive")
*   [Installation on Windows](#Installation_on_Windows "Installation on Windows")
    *   [Using .msi installer](#Using_msi_installer "Using .msi installer")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

GO is available to be installed on Win, Mac, and Linux platforms. You can download the binary of the latest GO version from the GO’s official download page – [https://golang.org/dl/](https://golang.org/dl/)

GO is available to be installed on Win, Mac, and Linux platforms.

# **Installation on MAC**

GO can be installed on MAC in three ways

*   Using archive
*   Using brew
*   Using .pkg installer

Let’s look at all the three ways

## **Using .pkg installer**

**Install**

*   Download the MAC pkg installer from here – [https://golang.org/dl/](https://golang.org/dl/). Double click the .pkg file and follow the onscreen instructions. Once finished GO will be installed in the below directory.

```
/usr/local/go
```

*   The installer will also add ‘**/usr/local/go/bin’**  to your env PATH variable. This is the directory where the GO binary resides. Relaunch terminal for changes to take effect

**Test Installation:**

*   Relaunch the terminal and type the command **‘which go’** in the terminal.It will output /usr/local/go/bin/go.  This is the location of go binary.

*   Try running the **‘go version’** command. It will output the current GO Version

**Uninstall**

To uninstall do below two steps:

```
rm -rf /usr/local/go      //Will require sudo permission
rm -rf /etc/paths.do/go   //Will require sudo permission. This action deletes will remove /usr/local/go/bin from PATH env
```

## **Using archive**

**Install**

*   Download the archive of the latest version of GO from here  – [https://golang.org/dl/](https://golang.org/dl/). After downloading untar it at the location **/usr/local.** You can run the below command to untar

```
tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz
```

*   After the untar, the below path will contain the go binary **‘/usr/local/go/bin’**. You have to add this location to your **.bashrc**. Open your **.bashrc** make the below entry

```
export PATH=$PATH:/usr/local/go/bin
```

**Test Installation**

*   Relaunch the terminal and type the command ‘which go’ in the terminal. It will output /usr/local/go/bin/go.  This is the location of the GO binary.

*   Try running the **‘go version’** command. It will output the current GO Version

*   Try running **‘go’** command as well. It will output

```
Go is a tool for managing Go source code.

Usage:

go <command></command> [arguments]
.....
```

**Uninstall**

To uninstall perform below two steps

*   Run below command to remove the files. It will require sudo permission

```
rm -rf /usr/local/go 
```

*   Remove the below entry from the **.bashrc** file

```
export PATH=$PATH:/usr/local/go/bin
```

## **Using brew**

**Install**

Simplest way to install GO on MAC is using the brew.

```
brew install go
```

**Test Installation**

*   Relaunch the terminal and type command **‘which go’** in the terminal.It will output **/usr/local/go/bin/go.** This is the location of GO binary.

*   Try running the **‘go version’** command. It will output the current GO Version

**Uninstall**

To uninstall simple run command

```
brew uninstall go
```

# **Installation on Linux and Free BSD**

## **Downloading the archive**

**Install**

*   Download the archive of latest version of GO from here  – [https://golang.org/dl/](https://golang.org/dl/). After downloading untar it at the location **/usr/local.** You can also run the below command to untar

*   After the untar then the below path contains the go binary **‘/usr/local/go/bin’** . You have to add this location to your .bashrc. Open your .bashrc make the below entry. If the file doesn’t already exist then create it.

**Test Installation:**

*   Relaunch the terminal and type the  command ‘which go’ in the terminal.It will output **/usr/local/go/bin/go.** This is the location of GO binary.
*   Try running the **‘go version’** command. It will output the current GO Version
*   Try running **‘go’** command as well.

**Uninstall**

To uninstall do below two steps

*   Run below command to remove the files. It will require sudo permission.

*   Remove the below entry from the .bashrc file

```
export PATH=$PATH:/usr/local/go/bin
```

# **Installation on Windows**

## **Using .msi installer**

**Install**

*   Download the .msi  installer from here – [https://golang.org/dl/](https://golang.org/dl/). Double click the .msi file and follow the onscreen instructions. Once finished GO will be installed in the below directory.

```
c:\Go

```

*   The installer will also add **‘c:\Go\bin’** directory in your PATH environment variable. This is the directory where the GO binary resides. You have to restart command prompt for changes to take effect

**Test Installation**

*   Try running the **‘go version’ **command. It will output the current GO Version
*   Try running **‘go’** command as well.

**Uninstall**

To uninstall do below two steps

*   Delete the directory** – c:\Go **
*   Remove the entry **‘c:\Go\bin’ **from the PATH env variable. See this link – [http://johnatten.com/2014/12/07/adding-and-editing-path-environment-variables-in-windows/](http://johnatten.com/2014/12/07/adding-and-editing-path-environment-variables-in-windows/)

# **Conclusion**

This is all about golang installation. Hope you have liked this article. Please share feedback or mistakes or improvements in comments

**Next Tutorial** – [Set up Go Workspace and hello world program](https://golangbyexample.com/workspace-hello-world-golang)
**Previous Tutorial** – [About Golang](https://golangbyexample.com/about-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*