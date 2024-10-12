<!--yml
category: 未分类
date: 2024-10-13 06:33:41
-->

# HTTP- Understanding multipart/form-data content-type

> 来源：[https://golangbyexample.com/multipart-form-data-content-type-golang/](https://golangbyexample.com/multipart-form-data-content-type-golang/)

In HTTP context, the **multipart/form-data** content-type is used for submitting HTML form. In the case of **multipart/form-data**, as the name suggests the body consists of different parts separated by a delimiter or boundary where each part is described by its own headers. The delimiter or boundary is also sent as part of the header only.

When you sending an HTML form through a browser in an HTTP call, the data contents can be sent in as request body below two formats.

*   application/x-www-form-urlencoded

*   multipart/form-data

For most of the cases, **application/x-www-form-urlencoded** can be used. **application/x-www-form-urlencoded** is not much efficient for

*   Sending Files or images

*   Sending large quantities of binary data or text which contains non-ASCII characters

For example, let’s say that the below data needs to be sent.

*   Name
*   Age

Then **application/x-www-form-urlencoded** can be used to send the above data. But let’s say that you also need to send the profile photo of the user in the request as well. So the data is now as below

*   Name
*   Age
*   Photo

In the above case, it will not be efficient to use **application/x-www-form-urlencoded** content-type. **multipart/form-data** should be used in this case. So for sending simple form data use **application/x-www-form-urlencoded** but if the form-data also needs to send binary data then use **multipart/form-data**

Why is that?.  We will answer this question once we have understood the format of **application/x-www-form-urlencoded** and **multipart/form-data. **

**application/x-www-form-urlencoded** encodes each non-ASCII byte to 3 bytes. Basically **applcation/x-www-form-urlencoded** content-type request body is like a giant query string. Similar to the query string in a URI it is a key-value pair having the below format

```
key1=value1&key2=value21&key2=value22&key3=value3
```

While sending **application/x-www-form-urlencoded**, all the non-alphanumeric characters are URL encoded. All the non-alphanumeric character(reserved only) will be URL encoded in the below format

```
%WW
```

Where **WW** is the ASCII code of the alphanumeric character represented in hexadecimal format.  As all the non-alphanumeric characters in the binary data are URL encoded where 1 byte is converted to 3 bytes. So size is increased three folds.  So if you sending a file or image which is a lot of binary data then your payload will be very big i.e almost thrice the size of the actual payload. Hence it is inefficient for sending large binary files or large NON-ASCII data.

To fully understand the format of **application/x-www-form-urlencoded** please refer to this link

Now let’s understand the format of **multipart/form-data.**

As we mentioned before as well **multipart/form-data** has different parts separated by a delimiter or a boundary. Each part is described by its own header. The format of multipart/form-data is as below

```
-- <delimiter_or_boundary>Content-Disposition: form-data; name="<key1>"
Content-Type: <content-type>

[DATA]
-- <delimiter_or_boundary>Content-Disposition: form-data; name="<key2>"; filename="<filename>"
Content-Type: <content-type>
[DATA]
--<delimiter_or_boundary>--</delimiter_or_boundary></content-type></filename></key2></delimiter_or_boundary></content-type></key1></delimiter_or_boundary>
```

As you can notice above that format starts with a delimiter or boundary and ends with a delimiter or boundary. The above format is divided into two parts. Also,

*   Each part is separated by a delimiter or boundary.

*   Each part contains its own headers to describe the type of data

*   Content-Disposition header for each part will be **form-data.** Contains the name field. This field contains the key name. If the part is a file  and it will also a **filename** field

*   Each part will also contain its own data.

Content-Disposition header for each part will be **form-data.** If you are sending binary data then 

Let’s say we are sending the below data as part of **multipart/form-data** request

*   name = John

*   age =23

*   photo = Some binary data

And let’s say the delimiter or boundary is

```
xyz
```

Then the format will be as below

```
--xyz
Content-Disposition: form-data; name="name"
Content-Type: text/plain

John
--xyz
Content-Disposition: form-data; name="age"
Content-Type: text/plain

23
--xyz
Content-Disposition: form-data; name="photo"; filename="photo.jpeg"
Content-Type: image/jpeg

[JPEG DATA]
--xyz--
```

As **multipart/form-data** will send the binary data as it is, that is why it is used for sending files and large binary data. Now the question is. Why not use form-data all the time then?

The reason is that for small data the additional requirement of boundary string and headers will outweigh any optimizations. For example, let’s say we have the below data to be sent

*   name=John

*   age=23

Then while using **application/x-www-form-urlencoded** the data will be sent as

```
name=John&age=23
```

But while sending **multipart/form-data** the data will be sent as below which is almost 10 times the data that is sent in **application/x-www-form-urlencoded**

```
--xyz
Content-Disposition: form-data; name="name"
Content-Type: text/plain

John
--xyz
Content-Disposition: form-data; name="age"
Content-Type: text/plain

23
--xyz--
```

**Conclusion**

This is all about multipart/form-data. Hope you have liked this article. Please share feedback in the comments