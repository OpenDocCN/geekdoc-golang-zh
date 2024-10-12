<!--yml
category: 未分类
date: 2024-10-13 06:31:38
-->

# Canonical HTTP Header Key Meaning

> 来源：[https://golangbyexample.com/canonical-http-header-key/](https://golangbyexample.com/canonical-http-header-key/)

Canonical form means that first character and after that any character following a hyphen is in uppercase. All other characters will be in lowercase. Example of canonical forms are

```
Content-Type
Accept-Encoding
```

All the keys in the headers are represented in the canonical form only. For eg if there are below headers in an incoming http request

```
content-type: applcation/json
accept-encoding: gzip
```

Then at server most of the http library in different languages at server will interpret it in the canonical form. For example the above headers will be as below when we print headers at server end.

```
Content-Type: application/json
Accept-Encoding: gzip
```

Note that

*   **content-type** is converted to canonical form **Content-Type**

*   **accept-encoding** is converted to canonical form **Accept-Encoding**