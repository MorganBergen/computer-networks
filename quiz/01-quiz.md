#  quiz 01

#  chapter 2

##  the application layer

###  overview
-  principles of network applications
-  web and http
-  e-mail, smtp, imap
-  the domain name system dns
-  p2p applications
-  video streaming and content distribution networks
-  socket programming with udp and tcp

**goals**
-  conceptual, implementation aspects of application-layer protocols
    +  transport-layer service models
    +  client-server paradigm
    +  peer-to-peer paradigm
-  learn about protocols by examining popular application-layer protocols and infrastructure
    +  http -  hypertext transfer protocol
    +  smtp -  simple mail transfer protocol
    +  imap -  internet mail access protocol
    +  dns -  domain name system
    +  cdns  -  content distribution networks
    +  video streaming systems
-  programming network application
    +  socket api

###  some network applications
-  social networking
-  web
-  text messaging
-  e-mail
-  multi-user network games
-  streaming stored video (youtube, hulu, netflix)
-  p2p file sharing
-  voice over ip (skype)
-  real-time video conferencing (zoom)
-  internet search
-  remote login

###  creating a network application
**write programs that:**
-  run on (different) end systems
-  communicate over network
-  web server software communicates with browser software

**no need to write software for network-core devices**
-  network-core devices do not run user applications
-  applications on end systems allows for rapid app development, propagation

###  client-server paradigm

**server:**
-  always-on host
-  permanent ip address
-  often in data centers, for scaling

**clients:**
-  contact, communicate with server
-  may be intermittently connected
-  may have dynmic ip addresses
-  do not communicate directly with each other
-  examples:  http, imap, ftp

###  peer-peer architecture
-  no always on server
-  arbitrary end systems directly communicate
-  peers request service from other peers, provideo service in return to other peers
    +  self scalability - new peers bring new service capacity, as well as new service demands
-  peers are intermittently connected and change ip addresses
    +  complex management
-  example:  p2p file sharing [bittorrent]

###  processes communicating
**process:**  program running within a host
-  within same host, two processes communicate using **inter-process communication** (defined by os)
-  processes in different hosts communicae by exchanging **messages**
-  note that applications with p2p architectures have client processes and server processes

**client process:** process that initiates communication

**server process:**  process that waits to be contacted

###  sockets 
-  process sends/receives messages to/from its **socket**
-  socket analogous to door
    +  sending process shoves message out door
    +  sending process relies on transport infrastructure on other side of door to deliver message to socket at receiving process
    +  two sockets involved:  one on each side

###  addressing processes
-  to receive messages, process must have identifier
-  host device has unique 32-bit ip address
-  **question**  does ip address of host on which process runs suffice for identifying the process?
    +  **answer**  no, many processes can be running  on same host
-  **identifier** includes both **ip address** and **port numbers** associated with process on host
-  example port number
    +  http server: 80
    + mail server: 25
-  to send http message to `gaia.cs.umass.edu` web server
    +  **ip address:**  128.119.245.12  
    +  **port number:** 80

###  an application-layer protocol defines
-  **types of messages exchanged**, 
    +  e.g., request, response messages
-  **message syntax:**
    +  what fields in messages & how fields are delineated
-  **message semantics**
    +  meaning of information in fields
-  **rules** for when and how processes send & respond to messages
**open protocols:**
    +  defined in request for comments (rfcs)

###  what transport service does an app need?

**data integrity**


1. What are the differences between the client-server approach and the
peer-to-peer approach?
2. What are the differences between TCP and UDP?
3. What is HTTP, and what is its primary purpose in the context of the
World Wide Web?
4. What is the purpose of a cookie in computer networking?
5. Can you mention some application layer protocols and transport
layer protocols that we discussed in class, along with their
abbreviations?