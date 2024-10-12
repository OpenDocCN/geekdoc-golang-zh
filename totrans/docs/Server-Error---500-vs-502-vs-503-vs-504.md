<!--yml
category: 未分类
date: 2024-10-13 06:09:11
-->

# Server Error – 500 vs 502 vs 503 vs 504

> 来源：[https://golangbyexample.com/server-error-5xx/](https://golangbyexample.com/server-error-5xx/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [500 (Internal Server Error)](#500_Internal_Server_Error "500 (Internal Server Error)")
*   [502 (Bad Gateway Error)](#502_Bad_Gateway_Error "502 (Bad Gateway Error) ")
*   [503 (Service Unavailable )](#503_Service_Unavailable "503 (Service Unavailable )")
*   [504(Gateway Timeout)](#504Gateway_Timeout "504(Gateway Timeout)")*  *# **Overview**

All 5XX errors are server errors. In this article, we will learn about the difference between all of them.

In this article, we use the term proxy/gateway server. Some of the examples of proxy/gateway server are:

*   nginx
*   AWS ELB
*   Cloudflare – Security Tool
*   API Gateway such as kong

# **500 (Internal Server Error)**

500 means Internal Server Error.  A service can raise a 500 in the case in which it ran into an error that was unexpected. Since 500 is purely a server error, it is meant to be retried by the client

**Some of the cases:**

*   An unhandled exception was generated at the app layer which was sent as 500.

*   App server got an invalid response from the database, due to which it was unable to complete the request and raised a 500.

**How to resolve:**

*   Look at the stack trace of the failing 500 call

# **502 (Bad Gateway Error)**

502  means that the proxy/gateway server in between the origin server and the client got an unexpected response from the origin server.  Do notice that in this case proxy/gateway server did get a response but an expected one. An example of an unexpected response could be that proxy/gateway server was expecting a JSON but instead got an HTML. It can happen in the below case

**Some of the cases:**

*   This could in cases lets say a firewall is blocking the connection between  proxy/gateway server and the origin server.  The firewall is returning a response which proxy/gateway server is not able to understand

*   Some network errors such as routing problems, DNS errors can also cause a 502.

*   Some bad programming can also cause a 502\. Let’s origin server returned an invalid HTML body which proxy/gateway server is not able to understand and the proxy server returns a 502\.

**How to resolve:**

*   Go through the logs to find out what was the bad response that was sent upstream which caused 502.

*   Reload the page

# **503 (Service Unavailable )**

503 means that the server is at capacity and it is not able to handle more request or it could also mean that the server is unavailable right now because it is under maintenance. It is generally a temporary state. Some of the cases can be

**Some of the cases:**

*   The server is at capacity and it refuses to accept any more connections. The traffic at the site is more than it can handle

*   The server has crashed and not able to accept connections.

*   The server is in maintenance mode.

**How to resolve**

*   Scale your servers as they are at full capacity

*   Check if your servers haven’t crashed

# **504(Gateway Timeout)**

504 means that the proxy server or gateway did not receive a response from the origin server within a specified timeout. Consider for example that an Elastic Load Balancer is sitting between your origin server, and the ELB timed out trying to receiving the response from your server. Some cases can be

**Some of the cases:**

*   Too many concurrent requests on your origin server can cause some of the requests to not complete within the specified time resulting in a 504 error at the proxy server end. If you seeing 504 consistently then that would mean a network issue between the proxy server and the origin server.
*   A 504 can also come when there was an unhandled exception at the origin server and it was not able to correctly return 500 back, instead it did not return anything, resulting in timeout at the proxy server end. In this scenario, there might not be too many concurrent requests but still
*   Network error can also cause the 504\. If you consistently getting the 504 and there is no anomaly observed at the server end of it the request is not even reaching the server, there are good chances that the upstream server is getting timeout because of a network error.

**How to resolve**

*   Increase the timeout at the proxy server end

*   Scale your servers as there might be too many concurrent requests and the server is not able to handle that.

*   Check for any errors at your server end in which it is not returning any response.

*   [500](https://golangbyexample.com/tag/500/)*   [502](https://golangbyexample.com/tag/502/)*   [503](https://golangbyexample.com/tag/503/)*   [504](https://golangbyexample.com/tag/504/)*   [server error](https://golangbyexample.com/tag/server-error/)*