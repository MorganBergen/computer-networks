#  application layer overview

**topics include**
-  principles of network applications
-  web and http
-  e-mail, smtp, imap
-  the domain name system dns
-  p2p applications
-  video streaming and content distribution networks
-  socket programming with udp and tcp

###  application layer:  overview

-  coneptual and implementation aspects of application layer protocols
    +  transport layer service models
    +  client server paradigm
    +  peer to peer paradigm

-  learn about protocols by examining popular application layer protocols and infrastructure
    +  http -  hyper text transfer protocol
    +  smtp -  simple mail transfer protocol
    +  imap -  internet message access protocol
    +  dns -  domain name system
    +  cdns - content delivery network services

-  programming network applications
    +  socket api

###  some network apps

-  social networking
-  web
-  text message
-  e-mail
-  multi user network games
-  streaming stored video
-  p2p file sharing
-  voice over ip
-  real time video conferencing
-  internet search
-  remote login

###  creating a network app

**write programs that**
-  run on different end systems
-  communicate over network
-  e.g. web server software communicates with browser software

**no need to write software for network core devices**
-  network core devices do not run user applications
-  applications on end systems allows for rapid app development propagation

###  client server paradigm

**server**
-  always on host
-  permanent ip address
-  often in data centers for scaling

**clients**
-  contact, communciate with server
-  may be intermittently connected
-  may have dynamic ip addresses
-  do not communicate directly with each other 
-  examples:  http, imap, ftp

###  peer-peer architecture

-  no always on server
-  arbitrary end systems directly communciaye
-  peers request service from other peers, provide service in return to other peers
    +  self scalability - new peers bring new service capacity, as well as new service demands
-  peers are intermittently connected and change ip addresses
    +  complex management
-  example: p2p file sharing such as bittorrent

###  process communciating

**process**:  program running within a host
-  within same host, two processes communciate using inter-process communcation defined by the operating system
-  processes in different hosts communicate by exchanging messages

**clients, servers**
-  **client process**:  process that initiates communication
-  *server process**:  process that waits to be contacted

-  note:  applications with p2p architectures have client processes & server processes

###  sockets

-  process sends/receives messages to/from socket
-  socket analogous to door
    +  sending process shoves messages out the door
    +  sending process relies on transport infrastructure on other side of door to deliver message to socket at receiving process
    +  two sockets involved:  one on each side

###  addressing processes

-  to receive messages, process must have an **identifer**
-  host device has a unique 32-bit ip address

-  question:  does ip address of host on which process runs suffice for identifying the process?
    +  answer:  no many processes can be running on same host

-  **identifier** includes both **ip address** and **port number** associated with process on host
-  example port numbers
    +  http server: 80
    +  mail server: 25

-  to send http messages to `gaia.cs.umass.edu` web server
    +  **ip address**:  128.119.245.12
    +  **port number**: 80

###  an application layer protocol defines

-  **types of messages exchanged**
    +  request, response

-  **message syntax**
    +  what fields in messages & how fields are delineated

-  **message semantics**
    +  meaning of information in fields

-  **rules**
    +  for when and how processes send & response to messages

**open protocols**
-  defined in request for comments (rpcs), everyone has access to protocol definition
-  allow for interoperability
-  e.g. https, smtp

**proprietary protocols**
-  e.g. skype, zoom

###  what transport service does an app need?

**data integrity**
-  some apps (e.g. file transfer, web transactions) require 100% reliable data transfer
-  other apps (e.g. audio) can tolerate some loss

**timing**
-  some apps (e.g. internet telephony, interactive games) require low delay to be effective

**throughput**
-  some apps (e.g. multimedia) require minimum amount of throughput to be effective
-  other apps "elastic apps" make use of whatever throughput they get

**security**
-  encryption, data integrity

###  transport service requirements:  common apps

|  application            | data loss       | throughput                                      | time sensitive? |
|:------------------------|:----------------|:------------------------------------------------|:----------------|
| file transfer/download  | no loss         | elastic                                         | no              |
| e-mail                  | no loss         | elastic                                         | no              |
| web documents           | no loss         | elastic                                         | no              |
| real-time audio/video   | loss-tolerant   | audio: 5kbps - 1mbps, video: 10 kbps - 5mbps    | yes, 10's msec  |
| streaming audio/video   | loss-tolerant   | audio: 5kbps - 1mbps, video: 10 kbps - 5mbps    | yes, few sec    |
| interactive games       | loss-tolerant   | kbps+                                           | yes, 10' msecs  |
| text messaging          | no loss         | elastic                                         | yes and no      |

###  internet transport protocol services

**tcp service**
-  **reliale** transport between sending and receiving process
-  **flow control**:  sender won't overwhelm receiver
-  **congestion control**:  throttle sender when the network is overloaded
-  **connection-oriented**:  setup required between client and server processes
-  **does not provide**: timing, minimum throughput guarantee, security

**udp service**
-  **unreliable data transfer** between sending and receiving process
-  **does not provide**: reliability, flow control, congestion control, timing, throughput guarantee, security, or connection setup

###  internet applications, and transport protocols

| application           | application layer protocol | transport protocol |
|:----------------------|:---------------------------|:-------------------|
| file transfer/download| ftp                        | tcp                |
| e-mail                | smtp                       | tcp                |
| web documents         | http                       | tcp                |
| internet telephony    | sip, rtp, or proprietary   | tcp or udp         |
| streaming audio/video | http, dash                 | tcp                |
| interactive games     | wow, fps, proprietary      | udp or tcp         |

###  securing tcp

**vanilla tcp & udp sockets**
-  no encryption
-  cleartext password sent into socket traverse internet in cleartext

**transport layer security tls**
-  provides encrypted tcp connections
-  data integrity
-  end-point authentication

**tls implemented in application layer**
-  apps use tls libraries, that use tcp in turn
-  cleartext sent into socket traverse internet encrypted

### web and http

-  web page consists of **objects**, each of which can be stored on different web servers
-  object can be html file, jpeg image, java applet, audio file,...
-  web page consists of **base html-file** which includes **several reference objects**, each addressable by **url**

- `www.someschool.edu/somedept/pic.gif`
- `hostname/pathname/pathname`

###  http overview

**http:  hypertext transfer protocol**
-  web application layer protocol
-  client/server model
    +  **client**:  browser that requests, receives, (using http protocol) and "displays" web objects
    +  **server**:  web server sends (using http protocol) objects in response to requests

**http uses tcp**
-  client initiates tcp connection, creates a socket to server on port number 80
-  server accepts tcp connection from client
-  http messages (application-layer protocol essages) exchanged between browser (http client) and web server (http server)
-  tcp connection closed

**http is stateless**
-  server maintains no information about past client requests

**protocols that maintain state are complex**
-  past history (state) must be maintained
-  if server/client crashes, their views of state may be inconsistent, must be reconciled

###  http connections: two types

**non-persistent http**
1.  tcp connection opened
2.  at most one object sent over tcp connection
3.  tcp connection closed

downloading multiple objects request multiple connections

**persistent http**
-  tcp connection opened to a server
-  multiple objects can be sent over a single tcp connection between client, and that server
-  tcp connection closed

###  non-persistent http: example

**user enters url:  `www.school.edu/department/home.index`**

1.  http client initiates tcp connection to http server (process) at `www.school.edu` on port `80`

1.  http server at host `www.school.edu` waiting for tcp connection at port `80` accepts connection, notifying client

3.  http client sends http **request message** containing url into tcp connection socket.  message indicates that client wants object `department/home.index`

4.  http server receives request message, forms **response message** containing request object, and sends message into its socket

5.  http server closed tcp connection

6.  http client receives response message containing html file, displays html.  parsing html file, finds 10 referenced jpeg objects

7.  steps 1 - 6 repeated for each of 10 jpeg objects

###  non-persistent http: resonse time

**rtt**
-  time for a small packet to travel from client to server and back

**http response time per object**
-  one rtt to initiate tcp connection
-  one rtt for http request and first few bytes of http response to return 
-  object/file transmission time

non-persistent http response = 2rtt + file transmission time

###  persistent http

**non-persistent http issues**
-  requires 2 rtts per object
-  os overhead for each tcp connection
-  browsers often open multiple parallel tcp connections to fetch referenced objects in parallel

**persistent http**
-  server leaves connection open after sending response
-  subsequent http messages between same client/server sent over open connection
-  client sends requests as soon as it encounters a referenced object
-  as little as one rtt for all the referenced objects cutting the response time in half

###  http request message

-  two types of http messages:  **request**, **response**
-  **http request message**
    +  ascii (human readable format)

```HTTP
GET \index.html HTTP/1.1\r\h
Host: www-net.cs.umas.edu\r\n
User-Agent: Mozilla/5.0 (macintosh; Intel mac OS X
    10.15; rv:80.0) Gecko/20100101 Firefox/80.0 \r\n
Accept: text/html, application/xhtml+xml\r\n
Accepting-Language: en-us, en; q=0.5\r\n
Accept-Encoding: gzip, deflate\r\n
Connection: keep-alive\r\n
\r\n
```

**request line (`GET`, `POST`, `HEAD` commands)**
```HTTP
GET \index.html HTTP/1.1\r\h
```

**carriage return character** `\r`

**line-feed character** `\n`

**header lines**
```HTTP
Host: www-net.cs.umas.edu\r\n
User-Agent: Mozilla/5.0 (macintosh; Intel mac OS X
    10.15; rv:80.0) Gecko/20100101 Firefox/80.0 \r\n
Accept: text/html, application/xhtml+xml\r\n
Accepting-Language: en-us, en; q=0.5\r\n
Accept-Encoding: gzip, deflate\r\n
Connection: keep-alive\r\n
\r\n
```

**carriage return, line feed at start of line indicates end of header lines**
```HTTP
\r\n
```

###  http request message general format
```
method | sp | url | sp | version | cr | if
header field name | | value | cr | if |
~                                     |
~                                     |
header field name | | value | cr | if |
cr | if |
~                entire body          | 
~                                     |
```

**request line** `method | sp | url | sp | version | cr | if`

**header lines**
```
header field name | | value | cr | if |
~                                     |
~                                     |
header field name | | value | cr | if |
```

**body**
```
~                entire body          | 
~                                     |
```

###  other http request messages

**`POST` method**
-  web page often includes form input
-  user input sent from client to server in entity body of `HTTP` `POST` request message

**`GET` method**
-  for sending data to server
-  include data in url field of `HTTP GET` request message following a `?`
-  `www.somesite.com/animalsearch?monkey&banana`

**`HEAD` method**
-  requests headers only that would be returned if specified url were requested with an `HTTP GET` method

**`PUT` method**
-  uploads new file (object) to server
-  completely replaces file that exists at specified url with conent in entity body of `POST HTTP` request method

### http response message

```HTTP
HTTP/1.1200 OK
Date: Tue, 08 Sep 2020 00:53:20 GMP
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips
    PHP/7.4.9 mod_perl/2.0.11 Perl/v5.16.3
Last-Modified: Tue, 01 Mar 2016 18:57:50 GMT
ETag: "a5b-52d015789ee9e"
Accept-Ranges: bytes
Content-Length: 2651
Content-Type: text/html; charset=UTF-8
\r\n
data data data data data ...
```

status line (protocol status code status phrase)
`HTTP/1.1 200 OK`

**header lines**
```HTTP
Date: Tue, 08 Sep 2020 00:53:20 GMP
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips
    PHP/7.4.9 mod_perl/2.0.11 Perl/v5.16.3
Last-Modified: Tue, 01 Mar 2016 18:57:50 GMT
ETag: "a5b-52d015789ee9e"
Accept-Ranges: bytes
Content-Length: 2651
Content-Type: text/html; charset=UTF-8
\r\n
data data data data data ...
```

**data, e.g., requested HTML file**
```
...
```


### http response status codes

status code appears in 1st line in server-to-client response message
-  **200 OK** request succeeded, requested object later in this message
-  **301 moved permanently** requested object moved, new location specified later in this message in location: field
-  **400 bad request** request message not understood by server
-  **404 not found** requested document not found on this server
-  **505 http version not supported**

###  maintaining user/server state: cookies

websites and client browser use **cookies** to maintain some state between transactions

**four components**:

1.  cookie header line of http response message
2.  cookie header line in next http request message
3.  cookie file kept on user's host, managed by user's browser
4.  back-end database at web site

###  http cookies:  comments

**what can cookies be used for**:
-  authorization
-  shopping carts
-  recommendations
-  user session state (web email)

**challenge:  how do you keep state?**
-  **at protocol endpoints**:  maintain state at sender/receiver over multiple transactions
-  **in messages**: cookies in http messages carry state

**cookies and privacy**
-  cookies permit sites to learn a lot about you on their site
-  third party persistent cookies (tracking cookies) allow common identity (cookie value) to be tracked across multiple web sites

###  cookies: tracking a user's browsing behavior

cookies can be used to
-  track user behavior on given website (first party cookies)
-  track user behavior across multiple websites (third party cookies) without user ever choosing to visit tracker site
-  tracking may be invisible to user
    +  rather than displaying ad triggering `HTTP GET` to tracker, could be invisible link

third party tracking via cookies:
-  disabled by default in firefox, safari browsers
-  to be disabled in chrome browser by the end of 2024

###  web caches

**goal**:  satisfy client requests without involving origin server
-  user configures browser to point to a local web cache
-  browser sends all http requests to cache 
    + **if** object in cache: cache returns object to client
    + **else** cache requests object from origin server, caches received object, then returns object to client

###  web caches (aka proxy servers)

-  web cache acs as both client and server
    +  server for original requesting client
    +  client to origin server
-  server tells cache about object's allowable caching in response header

`cache-control: max-age=<seconds>`

`cache-control: no-cache`

**why web caching**
-  reduces response time for client request
-  cache is closer to client
-  reduces traffic on an institution's access link
-  internet is dense with caches
    +  enables poor content providers to more effectively delivery conetnt

### caching example

**scenario**
-  access link rate: 1.54 mbps
-  rtt from institutional router to server: 2 sec
-  web object size:  100k bits
-  avg request rate browsers to origin servers: 15 sec
    + average data rate to browsers: 1.50 mbps

**performance**
-  access link utilization = 0.97
-  lan utolization: 0.0015
-  end-end delay 
    + = internet delay + access link delay + lan delay
    + = 2 sec + minutes + u secs

###  option 1: buy a faster access link

**scenario**
-  access link rate: 154 mbps
-  rtt from institutional router to server: 2 sec
-  web object size: 100k bits
-  average request rate browser to origin servers: 15/sec
    +  average data rate to browsers: 1.50 mbps

**performance**
-  access link utilization = 0.0097
-  lan utilization = 0.0015
-  end-end delay 
    + = internet delay + access link delay + lan delay
    + 2 sec + milliseconds + u secs

cost: faster access link is expensive

### calculating access link utilization, end-end delay with cache

suppose cache hit rate is 0.4:
-  40% requests served by cache, with low (msec) delay
-  60% requests satisfied at origin
    
- rate to browsers over access link
    + = 0.6 * 1.50 mbps 
    + = 0.9 mbps

- access link utilization 
    + = 0.9 / 1.54 
    + = 0.58 means low (msec) queueing delay at access link

-  average end-end delay
    + = 0.6 * (delay from origin servers) 
    + \+ 0.4 * (delay when satisfied at cache)
    + = 0.6 (2.01) + 0.4 (~msecs)
    + = ~1.2 seconds

lower average end-end delay than with 154 mbps link and cheaper too!

###  browser caching: conditional `GET`

**goal**:  dondoesnt send object if browser has up to date cached version
-  no object transmission delay or use of network resources
-  **client**:  specify date of browser-cached copy in http request 

`if modified since date`

-  **server**:  response contains no object if browser-cached copy is up to date 

`HTTP/1.0 304 Not Modified`

###  http/2

**key goal**:  decreased delay in multi-object http requets

**http1.1**:  introduced multiple, piplined **GETS** over single tcp connection
-  server responds in order (fcfs: first come first served scheduling) to `GET` requests
-  with fcfs, small object may have to wait for transmission (head-of-line (HOL) blocking) behind large objects
-  loss recovery (retransmitting lost tcp segments) stalls object transmission

###  http/2

**key goal**:  decreased delay in multi-obecjt http requests

**http/2**:  increased flexibility at server in sending objects to clients
-  methods, status codes, most header fields unchanged from http 1.1
-  transmission order requested objects based on client-specified object priority 
-  push unrequested objects to client
-  divide objects into frames, schedule frames to mitigate hol blocking

###  http/2:  mitigating hol blocking

http 1.1:  client requests 1 large object (e.g. video file) and 3 smaller objects
-  objects delivered in order requested: $O_2, O_3, O_4$ wait behind $O_1$

http 2: objects divided into frames, frame transmission is interleaved
-  objects $O_2, O_3, O_4$ delivered quickly, $O_1$ slightly delayed

###  http/2 to http/3

http/2 over tcp connection means
-  recovery from packet loss still stalls all object transmissions 
    +  as in http 1.1, browsers have incentive to open multiple parallel tcp connections to reduce stalling, increase overall throughput
-  no security over vanilla tcp connection
-  http/3:  adds security, per object error and congestion control (more pipelining) over udp

###  e-mail

**three major components**
-  user agents
-  mail servers
-  simple mail transfer protocol:  smtp

**user agent**
-  aka mail reader
-  composing, editing, reading mail messages
-  e.g. outlook, iphone mail client
-  outgoing, incoming messages stored on server

**mail servers**
-  **mailbox** contains incoming messages for user
-  **message queue** of outgoing to be sent mail messages

**smtp protocol** between mail servers to send email messages
-  **client**:  sending mail server
-  **server** :  receiving mail server

###  scenario:  alice sends email to bob

1.  alice uses userger agent to compose an email message to bob
2.  alice's user agent sends message to her mail server using smtp; message is placed in a message queue
3.  client side of smtp at mail server opens tcp connection with bob's mail server
4.  smtp client sends alice's message over the tcp connection
5.  bob's mail server places the message in bob's mailbox
6.  bob invokes his user agent to read the message

###  smtp rfc

-  uses tcp to reliably transfer a email message from client (mail server is initiating connection) to sever, port 45
    +  direct transfer: sending server acting like a client to receiving server
-  three phases of transfer
    +  smtp handshaking 
    +  smtp transfer of messages
    +  smtp closure
-  command/response interaction (like http)
    +  commands:  ascii text
    +  response:  status code and phrase

```
client <-----------------------------------> server
client <-initate tcp connection -----------> server
client <-----------tcp connected initiated-> server
client <--------------220------------------> server
client <--------------HELO------------------> server
client <--------------250 hello-------------> server
client <------smtp transfer-----------------> server
```

###  sample smtp interaction

```
S: 220 hamburger.edu
C: HELO crepes.fr
S: 250 Hello crepes.fr, pleased to meet you
C: MAIL FROM: <alice@crepes.fr>
S: 250 alice@crepes.fr... Sender ok
C: RCPT TO: <bob@hamburger.edu>
S: 250 bob@hamburger.edu ... Recipient ok
C: DATA
S: 354 Enter mail, end with "." on a line by itself
C: Do you like ketchup?
C: How about pickles?
C: .
S: 250 Message accepted for delivery
C: QUIT
S: 221 hamburger.edu closing connection
```

###  smtp:  observations

**comparison with http**
-  http:  client pull, meaning you're pulling a file from a server
-  smtp:  client push, meaning you're pushing a file from a server
-  both have ascii command/response interaction, status codes
-  http:  each object encapsulated in its own response message
-  smtp:  multiple objects sent in multipart message
-  smtp uses persistent connections
-  smtp requires message (header & body) to be in 7-bit ascii
-  smtp server uses crlf.crlf (new line feed) to determine end of message

###  mail message format

**smtp**:  protocol for exchanging email messages, defined in rfc 5321 (like rfc 7231 defines http)

**rfc 2822**  defines syntax for an email message itself like html defines syntax for web documents

```
header lines
-  to
-  from
-  subject
-  body
```

###  retrieving email:  mail access protocols

-  **smtp**:  delivery storage of email messages to receiver's server
-  mail access protocol:  retrieval from server
    +  **imap**:  internet mail access protocol:  messages stored on server, impa provides retrieval, deletion, folders of stored messages on server
-  **http**:  gmail, hotmail, yahoo mail, etc. provides web-based interface on top of smtp to send, imap or pop to retrieve email messages


###  dns:  domain name system

**internet hosts, routers**
-  ip address (32 bit) used for addressing datagrams
-  name, e.g. `www.wichita.edu` used by humans

**domain name system dns**
-  distributed database implemented in hierarchy of many name server
-  application layer protocol:  hosts, dns servers communicate to resolve names (address/ name translation)
    +  note:  core internet function, implemented as application layer protocol
    +  complexity at network's edge

question:  how does one map between ip address and name, and vice versa?

###  dns:  services, structure

**dns services**
-  hostname to ip address translation
-  host aliasing
    +  canonical alias names
-  mail server aliasing
-  load distribution
    +  relicated web servers:  many ip addresses correspond to one name

question:  why not centralize dns?
-  single point of failure
-  traffic volume
-  distant centralized database
-  maintenance

answer:  it doesnt scale
-  comcast dns servers alone: 600 billion dns queries/day
-  akamai dns serves alone: 2.2 trillion dns queries/day

###  dns:  a distributed, hierarchial database

```
                  root dns servers
              /           |           \
        .com servers  .org servers  .edu servers      
          /   \              \           /   \
    yahoo.com  amazon.com    pbs.org  nyu.edu  umass.edu
```

client wants ip address for `www.amazon.com` 1st approximation
-  client queries root server to find `.com` dns server
-  client queries `.com` dns server to get `amazon.com` dns server
-  client queries `amazon.com` dns server to get ip address for `www.amazon.com`

###  dns:  root name servers
-  official, contact-of-last-resort by name servers that can not resolve name
-  incredibly important internet function
    +  internet couldn't function without it
    +  dnssec - provides security authentication, message integrity
-  icann internet corporation for assigned names and numbers manages root dns domain
-  13 logical root name servers worldwide each server replicated many time 200 servers in us

###  top level domain and authoritative servers

**top level domain tld servers**
-  responsible for `.com`, `.net`, `.edu`, `.aero`, `.museumes`, and all top level country domains, e.g. `.cn`, `.uk`
-  network solutions: authoritative registry for `.com`, `.net` tld

###  dns records

**dns** distributed database storing resource records (rr) 
-  rr fromat: `(name, value, type, ttl)`

**type=A**
-  `name` is hostname
-  `value` is ip address

**type=NS**
-  `name` is domain (e.g. foo.com)
-  `value` is hostname of authoritative name server for this domain

**type=CNAME**
-  `name` is alias name for some canonical real name
-  www.ibm is really servereast.backup2.ibm.com
-  `value` is canonical name

**type=MX**
-  `value` is name of smtp mail server associated with `name`

###  dns protocol messages

###  multimedia:  video

-  **cbr constant bit rate**:  video encoding rate is fixed
-  **vbr variable bit rate**:  video encoding rate changes as amount of spatial, temporal coding ranges
-  examples of standards:  include mpeg1, mpeg2, meg4
    +  mpeg1 (cd-rom) 1.5 mbps
    +  mpeg2 (dvd) 3-6 mbps
    +  mpeg4 (often used in internet) 64 kbps 12 mbps

###  streaming stored video

main challenges:
-  include server to client bandwidth will vary over time, with changing network congestion levels in house, access network, network code, video server
-  packet loss, delay due to congestion will delay playout, or result in poor video quality

steps involves:
1.  video recorded (e.g. 30 frames/sec)
2.  video sent (network delay occured and is fixed)
3.  video received, played out at client (30 frames/sec)

**streaming**:  at this time, client playing out early part of a video, while the server is still sending later part of the video.

###  streaming stored video:  challenges

-  **continuous playout constraint**:  during client video playout, playout timing must match original timing.
    +  but **network delays are variable**, so there will need to be a client-side buffer to match continuous playout constraint
-  other challenges:
    +  client interactivity:  pause, fast-forward, rewind, jump through video
    +  video packets may be lost, or retransmitted

###  streaming stored video:  playout buffering

-  **client side buffering and platout delay**:  compensate for network added delay, delay jitter

<img src="./figures/playout.png">

###  streaming multimedia:  dash

**dash  dynamic, adaptive streaming over http**

**server**
-  divides video file into multiple chunks
-  each chunk is encoded at multiple different rates
-  different rate encodings stored in different files
-  files are replicated in various cdn (content distribution networks) nodes
-  **manifest file** provides urls for different chunks, where a certain chuck is defined what timestamp it's located

**client**
-  periodically estimates server to client bandwidth
-  consulting manifest, requests one chunk at a time
    +  chooses maximum coding rate sustainable given current bandwidth
    +  can choose different coding rates at different points in time (depending on available bandwidth at time), and from different servers
-  **intelligence at client** clients determines
    +  **when** to request a chunk (so that buffer starvation, or overflow does not occur)
    +  **what encoding rate** to request (higher quality when more bandwidth is available)
    +  **where** to request a chunk (can request from url server that is close to client or has high available bandwidth)

**streaming video** = encoding + dash + playout buffering

###  content distribution networks cdns

**challenge**:  how to stream content (selected from millions of videos) to hundreds of thousands of simultaneous users?

-  **option 1**:  single, large "mega server"... doesnt scale
    +  single point of failure
    +  point of network congestion
    +  long and possibly congested path to distant clients

-  **option 2**:  store/serve multiple copies of videos at multiple geographically distributed sites (cdn)
    +  enter deep:  push cdn servers deep into many access networks.  where it's close to users.  akamani is an example of 240,000 servers deployed in > 120 countries
    +  bring home:  smaller number (10s) of larger clusters in pop's near access nets used by limelight.

###  how does netflix work?

-  netflix: stores copies of content at its worldwide openconnect cdn nodes
-  subscriber requests content, service provider returns manifest 
    +  using manifest, client retrieves content at highest supportable rate
    +  may choose different rate or copy if network path is congested

###  content distribution networks (cdns)

-  **ott** over the top of internet host to host communication as a service
-  **ott challenges**:  coping with a congested internet from the edge
    +  what content to play in which cdn node?
    +  from which cdn node to retrieve content? at which rate?

###  socket programming

**goal**:  learn how to build client/server applications that communicate using sockets

**socket**:  door between application process and end-end transport protocol.

<img src="./figures/socket.png">

**two socket types for two transport services**
-  **udp user datagram protocol**:  unreliable datagram
-  **tcp transmission control protocol**:  reliable, byte stream-oriented

**application example**:
1.  client reads a line of characters (data) from its keyboard and sends data to server
2.  server receives the data and converts characters to uppercase
3.  server sends modified data to client
4.  client receives modified data and displays line on its screen

###  socket programming with udp

**udp**:  no connection between client and server
-  no handshaking before sending data
-  sender explicitly attaches ip destination address and port # to each packet
-  receiver extracts sender ip address and port # from received packet

**udp**:  transmitted data may be lost or received out of order
**application viewpoint**:  udp provides unreliable transfer of groups of bytes ("datagram") between client and server processes

<img src="./figures/udp.png">

###  example app:  udp client

```python
#  include python's socket library
from socket import *
serverName = 'hostname'
serverPort = 12000

#  create udp socket
clientSocket = socket(AF_INET, SOCK_DGRAM)

#  get user keyboard input
message = input('Input lowercase sentence:')

#  attach server name, port to message; sent into socket
clientSocket.sendto(message.encode(), (serverName, serverPort))

#  read reply data (bytes) from socket
modifiedMessage, serverAddress = clientSocket.recvfrom(2048)

#  print out received string and close socket
print(modifiedMessage.decode())

client.Socket.close()
```

###  example app udp server

```python
from socket import *
serverPort = 12000

#  create udp socket
serverSocket = socket(AF_INET, SOCK_DGRAM)

#  bind socket to local port number 12000
serverSocket.bind(("", serverPort))

print('The server is ready to receive')

#  loop forever
while True:
    message, clientAddress = serverSocket.recvfrom(2048)
    modifiedMessage = message.decode().upper()
    serverSocket.sendto(modifiedMessage.encode(), clientAddress)
```

###  socket programming with tcp

**client must contact server**
-  server process must first be running
-  server must have created socket that welcomes client's contact

**client contacts server by**
-  creating tcp socket, specifying ip address, port number of server process
-  **when client creates socket**:  client tcp establishes connection to server tcp

-  when contacted by client, **server tcp creates a new socket** for the server process to communicate with that particular client
    +  allows server to talk with multiple clients
    +  client source port # and ip address used to distinguish clients

