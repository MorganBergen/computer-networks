#  cs 664 computer networks
## homework 2

date: monday june 17 2023

due: friday june 21 2023

---

**1a. What is HTTP, and what is its primary purpose in web communication? Compare and contrast the key features and improvements of HTTP/1, HTTP/2, and HTTP/3. Discuss how each protocol addresses performance, efficiency, security, and other aspects of web communication.**

The hyper text transfer protocol (HTTP) is an application layer protocol which is implemented in two programs:  a client program and a server program which execute on different end systems, and communciate to each other by exchanging http messages.  The primary purpose of HTTP is to transfer resources between a client and a server.  HTTP/1.1 is a text based protocol that is stateless and connectionless.  HTTP/2 is a binary protocol that is stateful and connection oriented.  HTTP/3 is a binary protocol that is stateful and connection oriented.  HTTP/2 and HTTP/3 are improvements over HTTP/1.1 in that they are more efficient, secure, and performant.  HTTP/2 and HTTP/3 use multiplexing to allow multiple requests and responses to be sent over a single connection.  HTTP/2 and HTTP/3 use header compression to reduce the size of the headers.  HTTP/2 and HTTP/3 use server push to send resources to the client before the client requests them.  HTTP/3 uses QUIC to provide security and performance improvements over HTTP/2.

**HTTP/1.1**
-  A text based protocol that supports `GET`, `POST`, `PUT`, `DELETE`, and `HEAD` methods that are used to request and send resources between a client and a server in plain text.  For example, a HTTP request message would look like this:

```HTTP
GET /index.html HTTP/1.1\r\n
Host:  www.example.com\r\n
User-Agent:  Mozilla/5.0 (Macintosh; Intel Mac OS X)
    10.15; rv: 80.0) Gecko/20100101 Firefox/80.0\r\n
Accept: text/html, application/xhtml+xml\r\n
Accept-Language: en-us, en; q=0.5\r\n
Accept-Encoding: gzip, deflate\r\n
\r\n
```

-  Each and every request opens a new connection which can cause latency and overhead.
-  When a client and server communciate a series of requests may be made back to back a periodically regular intervals, this is whewre the two persistent and non-persistent connections features differentiate from each other in their behavior of how request objects are transmitted.  
-  Chunked transfer encoding is used to send resources in chunks, which allows data to be sent in a series of blocks, which allows the server to start transmitting content before knowing the total size of the content.

**HTTP/2**
-  Binary based protocol making parsing more efficient and less prone to errors.
-  Multiplexing allows for multiple requests and responses to be simultaeously sent over a single connection.
-  Header compression helps reduce the often large and repetitive http headers, this is done by using a header table that stores the most common headers and their values, and then only sending the index of the header in the table.
-  Provides load control over streams and a tcp connection, this helps in managing resources by avoiding che response time in half.ongestion and overloading the network.
-  Improved efficiency with the use of network resoureces with better load times and the server push feature.

**HTTP/3**
-  Built on top of the udp protocol rather than the tcp protocol, allowing for faster connections and improved performance.
-  Reduced latency by combining connection establishment with a cryptographic handshake allowing data to be transmitted with fewer round trips.
-  Multiplexing without head of line blocking.  Head of line blocking is when a packet is lost or delayed, all packets behind it are also delayed.  Multiplexing without head of line blocking allows for packets to be sent and received out of order.
-  Encrypted connection by default enchange security and privacy.
-  Stream prioritization and dependency which allow clients to prioritize certain streams over others.

**1b.  Explain the concept of a persistent http connection and its advantages?  How does it differ from a non-persistent http connection?  Provide examples of scenarios where each type of connection may be beneficial.**

**Explanation of persistent http connection**

A persistent http connection is where a tcp connection opens to a server which allows for the client and server to send and receive multiple objects over a single tcp connection, then when transmission ends the tcp connection closes.  The advantage of a persistent http connection is that it has as little as one RTT (round trip time) for all referenced objects; this cuts the response time in half.  Subsequent requests and responses between the same client and server can be sent over the same connection.  There are two features in persistent http which is pipelining, multiplexing, and configurable timeout intervals.  Pipelining allows for requests for objects to be sent back to back without waiting for replies to pending requests.  Multiplexing allows for multiple requests and responses to be sent over a single connection.  And configurable timeout intervals is where the http server closes a connection when it isn't in use for a certain amount of time, closing these connections helps free up resources, allowing for the server to handle more active connections and handle requests more efficiently.


**Explanation of non-persistent http connection**

A non-persistent http connection is where a tcp connection opens to a server, sends a single object, then closes the tcp connection.  The advantage of a non-persistent http connection is that it is simple and easy to implement.  

**Example Scenarios**







































