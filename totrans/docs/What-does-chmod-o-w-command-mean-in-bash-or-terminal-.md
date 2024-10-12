<!--yml
category: 未分类
date: 2024-10-13 06:50:02
-->

# What does chmod o-w command mean in bash or terminal￼

> 来源：[https://golangbyexample.com/chmod-ow-command/](https://golangbyexample.com/chmod-ow-command/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Permission Groups](#Permission_Groups "Permission Groups")
*   [Permission Types](#Permission_Types "Permission Types")
*   [Operations Definitions](#Operations_Definitions "Operations Definitions")
*   [Example](#Example "Example")*  *## **Overview**

There are three components that are in the picture when it comes to managing the permissions of a file. 

## **Permission Groups**

*   **User** – Abbreviated by **‘u’**

*   **Group** – Abbreviated by **‘g’**

*   **Other**– Abbreviated by **‘o’**

*   **All** – Abbreaviated by **‘a’**

## **Permission Types**

*   **Read Permission –** Abbreviated by **‘r’**

*   **Write Permission –** Abbreviated by **‘w’**

*   **Execute Permission –** Abbreviated by **‘x’**

## ****Operations Definitions****

 ***   **+** is used to add a permission

*   **–** is used to remove a permission

*   **=** is used to set a permission

So **o-w** means taking away **write** permission from the **other** user

Before we see an example let’s see how file permissions are represented when you run **ls** command

![](img/b2cc19da19010470a5244967526784b2.png)

Below are some points about the above diagram

*   The first character represents the file type. ‘-‘ means regular file and ‘d’ means directory

*   The second to fourth character represents the Read, Write and Execute Permission for the owner.

*   The fourth to seventh character represents the Read, Write and Execute Permissions for the group

*   The eighth to the tenth character represents the Read, Write and Execute Permissions for the other user

## **Example**

*   Create a file **temp.txt**. Check its permissions.

```
ls -all | grep temp.txt
-rw-r--r--    1 root  root      0 Aug  9 14:50 temp.txt
```

Notice that the **other** user only has permission to only **read**

*   Now run command to give write permission to the other user

```
chmod o+w temp.txt
ls -all | grep temp.txt
-rw-r--rw-    1 root  root      0 Aug  9 14:50 temp.txt
```

See output above. Execute permission is also given to the other user.

*   Now run command to take away write permission to the other user

```
chmod o-w temp.txt
ls -all | grep temp.txt
-rw-r--r--    1 root  root      0 Aug  9 14:50 temp.txt
```

Notice how write permission is taken away from the other user***