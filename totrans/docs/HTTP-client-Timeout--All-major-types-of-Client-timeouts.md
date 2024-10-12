<!--yml
category: 未分类
date: 2024-10-13 06:07:11
-->

# HTTP client Timeout: All major types of Client timeouts

> 来源：[https://golangbyexample.com/all-types-client-timeouts-http-tcp-udp-unix/](https://golangbyexample.com/all-types-client-timeouts-http-tcp-udp-unix/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Dial Timeout:](#Dial_Timeout "Dial Timeout:")
*   [TLS Handshake Timeout:](#TLS_Handshake_Timeout "TLS Handshake Timeout:")
*   [ResponseHeader Timeout:](#ResponseHeader_Timeout "ResponseHeader Timeout:")
*   [Request Timeout:](#Request_Timeout "Request Timeout:")
*   [Socket Timeout:](#Socket_Timeout "Socket Timeout: ")
*   [Wait for Connection Timeout:](#Wait_for_Connection_Timeout "Wait for Connection Timeout:")
*   [Idle Connection Timeout:](#Idle_Connection_Timeout "Idle Connection Timeout:")*  *# **Overview**

In this article, we will study different types of timeouts that are possible in HTTP or TCP or UDP or Unix Sockets Communication, etc when connecting from **Client** to the **Server**.

There is also something called **ServerTimeout** that specifies when the server can close an incoming request. Just to add, the Server timeout exists to protect a server from the malicious client or a slow client which is holding a connection for very long.

For **Client Timeouts**  please note that different libraries in different programming languages may not give you an option to set all the timeouts which are described belowFor eg in golang net http package the Client struct ([https://golang.org/pkg/net/http/#Client](https://golang.org/pkg/net/http/#Client)) allows you to set a single **Timeout.** This timeout specifies below two timeouts

1.  Request Timeout
2.  Socket Timeout

Let’s learn all types of timeouts:

# **Dial Timeout**:

Dial Timeout specifies the maximum amount of time a dial will wait for a connect to complete on a  named network. The named network could be **“TCP”, “UDP” , “IP”, “unix”** etc. It is a case where no response at all is received from the server. It may happen because

*   Wrong DNS
*   The remote server does not respond
*   The remote server is dead
*   Wrong port used to connect to the remote server

**Example**

Imagine a case where DialTimeout is 1 sec. A client is dialing a TCP connection but does not receive any response from the server with 1 sec then DialTimeout will happen.

# **TLS Handshake Timeout**:

Applicable for SSL request. Whenever a connection is getting established with a remote server, a  TLS handshake happens in which SSL keys are exchanged and SSL certification verification of the server is done.  **TLS Handshake** **Timeout** specifiesthemax time to wait for TLS Handshake to be established.

**Example:**

Imagine a case of an HTTP request. Let’s say the TLS Handshake timeout is 1 sec. The Client  will max wait for 1 sec for the TLS Handshake to be completed

# **ResponseHeader Timeout**:

After the client has done the  TLS Handshake with the server and also done sending the request headers and request body fully then **ResponseHeader Timeout** comes into the picture. This timeout specifies the max time to wait for receiving response headers from the server.

**Example:**

Let’s say the Response HeaderTimeout is 1 second. A client makes an HTTP request, the TLS handshake is successful. The client then sends the request headers and request body to the server.  Now in this scenario, the **Response HeaderTimeout** will happen if server response headers are not received within 1 second

# **Request Timeout:**

Request Timeout comes into picture starting from dialing of a connection to the server to the receive of the entire response body from the remote server. It represents the max time to wait for entire client request to complete. Imagine an HTTP request, a request timeout specifies how much max time it will wait for the entire HTTP request to be completed. It is the timeout for the complete request so it includes **DialTimeout**, **TLSHandshakeTimeout** as well as **Response HeaderTimeout** and plus extra time waiting for response body.

**Example:**

Let’s say you Request Timeout is 1 sec, then the client will wait for max 1 sec starting from Dial to receive the entire response before timing out.

# **Socket Timeout:**

This timeout is mostly related to long-lived connections. A connection has been established with a remote server and some chunks of bytes also have been transferred. If this data flow of bytes is interrupted for a specific time, then the socket timeout comes into the picture.

**Example:**

Imaging your socket timeout is 1 sec and if no new bytes are transferred for the next 1 sec, then it will result in socket timeout. By setting the socket timeout to 1 sec means that some data should be received every one second. So a socket timeout is waiting for individual packets and it does not mean timeout for a full response.

# **Wait for Connection Timeout:**

This timeout is applicable for a fixed connection Pool. This is the timeout waiting for an active connection to be available from the pool.

**Example**:

Let’s take an example of a DB connection pool. Imagine you are using MYSQL server and max connections in a pool is 20 and all connections are currently utilized. Your **“Wait for Connection Timeout”** is set to 1 sec. A new request comes and it needs a connection to connect to DB. Since all 20 connections are occupied, then, in this case, the new request will wait for max 1 second for connection to be available before timing out. In this case, the response you might get is **“Too many connections”** error.

# **Idle Connection Timeout:**

This timeout is also applicable to a connection pool. Idle Connection Timeout specifies how much time an unused connection should be kept around.

**Example:**

Imaging a connection pool of 20 connections. At a particular time, 5 connections were getting used parallelly. But after that, there was no incoming request. So these 5 connections will wait for a value equal to “Idle Connection Timeout” before being released. An idle connection timeout of 1 sec should mean that a particular connection should not be idle for more than 1 second otherwise it will be released and a new connection will be creating for any new request.*