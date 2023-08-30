#  introduction to communication networks

<img src="https://github.com/MorganBergen/communication-networks/assets/65584733/9bd43b4f-5b75-47ff-bae8-6d4c59f5e749" width="700px" align="right">

####  contents

1.  [textbook](##textbook)
2.  [description](##description)
3.  [schedule](#schedule)
4.  [outline](##outline)
5.  [grading](##grading)

###  textbook

[computer networking: a top-down approach, 7th edition, by kurose and ross](https://www.ucg.ac.me/skladiste/blog_44233/objava_64433/fajlovi/Computer%20Networking%20_%20A%20Top%20Down%20Approach,%207th,%20converted.pdf)

###  description

an introduction to the principles used in communication networks is given.  topics include discussion used of communications networks, network traffic, network impairments, standards, layered reference models for organizing network functions.  local area network technology and protocols are discussed.  link, network, transport layer protocols, and security are introduced.  TCP/IP networks are stressed.  basic concepts of network performance evaluation and wireless communications are studied.  both analytical and simulation techniques are considered.

1.  basics of data communication and network architecture
2.  network layer control and protocol
3.  link layer control and protocols
4.  TCP/IP protocol suite
5.  high speed LANs
6.  internetworking technologies
7.  design advanced communication protocols

###  outline

####  1.  introduction, application, and transport layer (6 weeks)

-  introduction
    - internet: history, core, and edge

-  application layer
    -  application level protocols (HTTP, FTP, SMTP/POP3/IMAP, DNS)
    -  creating network applications (Socket API)

-  transport layer
    -  transport layer services
    -  multiplexing / demultiplexing
    -  connectionless transport:  UDP
    -  principles of reliable data transfer
    -  connection oriented transport TCP:  reliable transfer, flow control, connection management
    -  **midterm exam 1**
    -  principles of congestion control
    -  TCP congestion control

####  2.  network and link layer (5 weeks)

-  network layer
    -  network layer services
    -  internet protocol IP and addressing IPv4, IPv6
    -  routing algorithms:  link-state and distance-vector
    -  internet routing protocols:  Intra-domain, Inter-domain
    -  multicast and anycast routing
    -  **midterm exam 2**

-  link layer
    -  link layer services
    -  error detection, correction
    -  multiple access protocols, and LANs
    -  link layer addressing, ARP
    -  specific link layer technologies

####  3.  advanced topics (2 weeks)
-  wireless communications
-  wireless links, characteristics
-  IEEE 802.11 wireless LAN (Wi-Fi)
-  network coding
-  advanced variations of transport protocols (wireless, data center, etc.)

-  **final exam**

###  grading

| unit      | weight        |
|:-----------|:--------------|
| midterm 1  | 20%           |
| midterm 2  | 20%           |
| final      | 25%           |
| homework   | 25%           |
| quizzes    | 10%           |

late homework submission policy:  
-  one day late:  max 75% of grade
-  two days late:  max 50% of grade
-  three days late:  max 25% of grade
