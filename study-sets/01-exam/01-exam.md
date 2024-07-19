#  exam 1 concepts

1.  what is the purpose of a cookie in computer networking

a cookie is a small piece of data stored on a web browser on behalf of a website to maintain state information between transactions.  cookies are used for session management, maintaining state information, user authorization, tracking, and analytics.

2.  why is there udp?

user datagram protocol exists because it is simple, connectionless, has reduced latency, has lower overhead, and is stateless.  udp does not require any setup or handshake, and can function when a network is compromised.

3.  what is the role of a buffer in streaming video playback?  how does it contribute to a smooth and uninterrupted viewing experience?

the role of a buffer in streaming video playback is crucial for ensuring a smooth and uninterrupted viewing experience.  the buffer stores a portion of the video ahead of what's being watched.  it compensates for network issues by preloading data and adapting to variable network speeds.

4.  how does rdt (reliable data transfer) recover from errors?

acknowledgements:  the receiver explicitly tells the sender that the packet was received okay.

negative acknowledgements:  the receiver explicitly tells the sender that the packet had errors and that the sender needs to retransmit the packet.

5.  explain the request and response model in http.  what are the key components of an http request and http response?

the request and response mdodel in http is a way for clients to ask for information or actions from a server and recieve a reply.  

http request components:  method, uri, http version, headers, body

http response components:  status code, http version, headers, body

6.  what are the differences between tcp and udp?

tcp
-  reliable transport:  between sending and receiving processes
-  flow control:  sender won't overwhelm the server
-  congestion control:  setup required between client and server
-  does not provide:  timing, minimum throughput guarantee, and security

udp
-  unreliable data transfer:  between sending and receiving processes
-  does not provide:  reliability, flow control, congestion control, timing, throughput guarantee, and security

7.  compare and contrast the key features and improvements of http1, http2, and http3.  discuss how each protocol addresses performance, efficiency, security, and the aspects of web communication.

http1 -  uses a single connection for each request and response leading to potential latency issues.

http2 -  uses multiplexing, header compression, and enhances speed and efficiency

http3 -  uses the quic protocol instead of tcp, excels in high latency scenarios and ehances security and performance.

8.  explain the concept of persistent http connection and its advantages.  how does it differ from non-persistent http connection?  provide examples of scenarios where each type of connection might be beneficial 

persistent http connection enables multiple requests and responses to use a single network connection, reducing connection setup overhead.  it minimizes latency by avoiding the need to repeatedly establish connections.  fewer connections save server and network resources.  it is ideal for complex web pages, streaming services, and interactive applications where keeping a connection alive reduces latency and enhances user experience.

non persistent http connection opens a new connection for each request, leading to higher latency and less efficiency.  non persistent http connections work well for simple standalone requests and is ideal for simple web pages, resource-constrained serves, and security sensitive transactions where the overhead of connection reuse may not be necessary.

