#  computer networks and the internet

##  chapter 1.1

###  introduction

-  what is the internet?  what is a protocol?
-  **network edge**: hosts, access network, physical media
-  **network core**: packet/circuit switching, internet structure
-  **performance**:  loss, delay, throuput
-  protocol layers, service models
-  security and history

###  the internet:  a nuts and bolts view

**internet "network of networks"**
-  interconnected isps - internet service providers

**protocols** are everywhere control sending and receiving of messages
-  http, streaming video, skype, tcp, ip, wifi, 4/5g, and ethernet

**internet standards**
-  **rfc**:  request for comments
-  **ietf**:  internet engineering task

###  the internet: a services view

**infrastructure** that provides services to applications 
-  web, streaming video, multimedia, teleconferencing, emails, games, e-commerce, social media, interconnected applications

provides **programming interface** to distributed applications

-  "hooks" allowing sending/receiving apps to connect to, use internet transport service
- provides service options, anaogous to postal service


###  what's a protocol?

**network protocols** 
-  computers or devices rather than humans where communication activity in internet governed by protocols

 **protocols** define the format, order of messages sent and received among network entities, and action taken on message transmission, receipt 

-  rules for specified messages sent
-  or rules for specific actions taken, when messages received or for other events


```
host <--------------------------------------> server
host <---- __tcp connection request__ ------> server
host <---- __tcp connect reply__ -----------> server
host <---- `get http://www.person.com` -----> server
host <---- `<file>` ------------------------> server
```

**network edge**

-  hosts: clients and servers
-  servers are often in data centers

**access networks, physical meda**

-  wired, wireless communication links

**network core**

-  interconnected routers
-  network of networks

question:  how does one connect end systems to edge routers?

-  residential access networks
-  institutional access networks such as schools and companies
-  mobile access networks such as wifi, 4g and 5g

###  access networks:  cable-based access

-  **frequency division multiplexing fdm**:  different channels transmitted in different frequency bands
-  **hfc:  hybrid fiber coax**:  asymmetric:  up to 40 mbps - 1.2 gbps downstream transmission rate.
-  **network** of canble, fibers attaches home to isp routers, homes share access networks to cable headed

###  access networks:  digital subscriber line (dsl)

-  uses existing telephone line to central office dslam
-  data over dsl phone line goes to the internet
-  voice over dsl phone line goes to telephone net
-  24 - 25 mbps dedicated downstream transmission rate
-  3.5 16 mbps dedicated upstream transmission rate

###  access networks:  home networks

-  wireless and wired devices are connected to a router

**to / from headed or central office**
-  wifi wireless access point (54, 450 mbps):  often combined in a single box
-  wired ethernet (1 gbps)
-  router, firewall, nat
-  canble or dsl modem 

###  wireless access networks

-  shared wireless access network connected end systems to router via base station aka "access point"

**wireless local area networks wlans**
-  typically within or around building (~100 ft)
-  802.11 b/g/n (wifi):  11, 54, 450 mbps transmission rate

**wide-area cellular access networks**
-  provided by mobile, cellular network operator (10 km)
-  10's mbps
-  4g / 5g cellular networks

###  access networks:  enterprise networks

-  companies, universities, etc.
-  mix of wired, wireless link technologies, connecting a mix of switches and routers
-  ethernet:  wired access at 100 mbps, 1 gbps, 10 gbps
-  wifi:  wireless access points at 11, 54, 450 mbps

###  access networks:  data center networks

-  high bandwidth links (10s to 100s gbps) connect hundreds to thousands of servers together and to the internet.

###  host:  sends packets of data

host sending function
-  takes application message
-  breaks into small chunks known as __packets__, of l bits
-  transmits packet into access network at transmission rate r
-  link transmission rate, aka link capacity, aka link bandwidth

$$
\text{packet transmission delay} = \text{time needed to transmit l-bit packet into link} = \frac{l \text{ (bits)}}{r \text{ (bits/sec)}}
$$

###  links: physical media

-  **bit**:  propages between transmitter/receiver pairs
-  **physical link**:  what lies between transmitter & receiver
-  **guided media**:  signals propagate in solid media:  copper, fiber, coax
-  **unguided media**:  signals propagate freely, e.g. radio

**coaxial cable**
-  two concentric copper conductors
-  bidirectional
-  broadband:  multiple frequency channels on canble, 100s mbps per channel.

**fiber optic cable**
-  glass fiber carrying light pulses, each pulse a bit
-  high speed operation:  high-speed point to point transmission (10's - 100s gbps)
-  low error rate:  repeaters spaced far apart, immune to electromagnetic noise

**wireless radio**
-  signal carried in various "bans" in eletromagnetic spectrum
-  no physical "wire"
-  broadcast, "half duplex" (sender to receiver)
-  propagation environment effects:  reflection, obstruction by objects, and interference/noise 

**radio link types**

-  wireless lan (wifi):  10 - 100 mbps; 10's of meters
-  wide-area (e.g. 4g/5g cellular) 10's mbps (4g) over ~10 km
-  bluetooth:  cable replacement, short distances and limited rates
-  terrestrial microwave:  point to point; 45 mbps channels
-  satellite:  up to < 100 mbps (starlink) downlink, and 270 msec end-end delay (geostationary)

## the network core

-  mesh of interconnected routers
-  **packet-switching**:  hosts break application-layer messages into **packets**
-  network forwards packets from one router to the next, across links on path from source to destination

###  two key network core functions

**forwarding**
-  aka switching
-  location action:  move arriving packets from router's input link to appropriate router output link

**routing**
-  global action:  determine source-destination paths taken by packets
-  routing algorithms

###  packet-switching: store-and-forward

-  **packet transmission delay**:  takes L/R seconds to transmit (push out) L-bit packet into link at R bps
-  **store and forward**:  entire packet must arrive at router before it can be transmitted on next link

**one-hop numerical example**
-  L = 10 kbits
-  R = 100 mbps
-  one-hop transmission delay = 0.1 msec

###  packet-switching:  queuing

-  queueing occurs when work arrives faster than it can be serviced

**packet queuing and loss**:  if arrival rate (in bps) to link exceeds transmission rate (bps) of link for some period of time
-  packets will queue, waiting to be transmitted on output link
-  packets can be dropped (lost) if memory (buffer) in router fills up

###  alternative to packet switching:  circuit switching

**end-end resources allocated to, reserved for "call" between source and destination**
-  in diagram, each link has foru circuits
-  call gets 2nd circuit in top link and 1st circuit in right linkk
-  dedicated resources:  no sharing
-  circuit link (guaranteed) performance
-  circuit segment idle if not used by call (no sharing)
-  commonly used in traditional telephone networks






























