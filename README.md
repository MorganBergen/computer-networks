#  introduction to communication networks

<img src="https://github.com/MorganBergen/communication-networks/assets/65584733/9bd43b4f-5b75-47ff-bae8-6d4c59f5e749" width="500px" align="right">

##  contents

1.  [textbook](##textbook)
2.  [description](##description)
4.  [outline](##outline)

###  textbook

[computer networking: a top-down approach, 7th edition, by kurose and ross](https://www.ucg.ac.me/skladiste/blog_44233/objava_64433/fajlovi/Computer%20Networking%20_%20A%20Top%20Down%20Approach,%207th,%20converted.pdf)

###  description

this is an introduction to the principles used in communication networks is given.  

topics include discussion used of communications networks, network traffic, network impairments, standards, layered reference models for organizing network functions.  local area network technology and protocols are discussed.  link, network, transport layer protocols, and security are introduced.  TCP/IP networks are stressed.  basic concepts of network performance evaluation and wireless communications are studied.  both analytical and simulation techniques are considered.

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

####  2.  network and link layer

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

####  3.  advanced topics

-  wireless communications
-  wireless links, characteristics
-  IEEE 802.11 wireless LAN (Wi-Fi)
-  network coding
-  advanced variations of transport protocols (wireless, data center, etc.)

#  computer networks and the internet

|  **[back](./README.md)** |    |
|:-----------------------------|:---|
| **course** | introduction to communication networks |
| **covers** | the internet |

####  contents

1.  [what is the internet](#what-is-the-internet)
2.  [what is a protocol](#what-is-a-protocol)
3.  [network edge](#network-edge)
4.  [network core](#network-core)
5.  [performance](#performance)
6.  [protocol layers](#protocol-layers)
7.  [history](#history)

##  what is the internet?

the internet is a computer network that interconnect billions of computing devices throughout the world.

computing devices were primarily traditional desktop pcs, linux, workstations, and so called servers that store and transmit information such as web pages and email messages.

now non-traditional iot devices internet of things such as laptops, smartphones, tables, tv's, gaming consoles, thermostats, home security systems, home appliances, watches, eye glasses, cars, traffic control systems, and more are being connected to the internet.

devices are called hosts or end systems.  recorded in 2015 there were estimated to have been about 5 billion devices connected to the internet, and the number will reach 25 billion by 2020.  

###  key pieces of the internet

-  host ( = end system )
-  server
-  mobile
-  router
-  link-layer switch
-  modem
-  base station
-  smartphone
-  cell phone tower
-  tabley
-  traffic light
- thermostat
-  fridge
-  flat computer monitor
-  keyboard

end systens are connected together by a network of communication links and packet switches.

different links can transmit data at different rates, with the transmission rate of a link measured in bits/seconds.


###  important terminology

-  communication links
-  packet switches
-  transmission rate
-  packets
-  routers
-  link-layer switches
-  route
-  path
-  internet service providers
-  protocols
-  transmission control protocol (tcp)
-  internet protocol
-  tcp/ip
-  internet standards
-  requests for comments
-  distributed applications

##  a services description


















