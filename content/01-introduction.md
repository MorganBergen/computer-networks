 #  computer networks and the internet

##  chapter 1

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

### the network core

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

-  **packet transmission delay**:  takes l/r seconds to transmit (push out) l-bit packet into link at r bps

-  **store and forward**:  entire packet must arrive at router before it can be transmitted on next link

**one-hop numerical example**
-  l = 10 kbits
-  r = 100 mbps
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

![img_1458](https://github.com/morganbergen/computer-networks/assets/65584733/0da6ae94-f051-4511-90e0-3a10d5af7c41)


###  circuit switching:  fdm and tdm

**frequency division multiplexing fdm**
-  optical, eletromagnetic frequencies divided into narrow frequency bands
-  each call allocated its own band, can transmit at max rate of that narrow band

**time division multiplexing tdm**
-  time divided into slows
-  each call allocated periodic slots, can transmit at maximum rate of wider frequency band only during its time slots.

###  packet switching vs circuit switching

example:  
-  1 gb/s link
-  35 users
-  each user 100 mb/s when active & 10% of the time

question:  how many users can use this network under circuit switching and packet switching

-  circuit switching: 10 users
-  packet switching:  with 35 users, probability > 10 active at same time is less tahn 0.0004

**is packet switching a winner?**

-  great for bursty data - sometimes has data to send, but at other times not for resource sharing, and simpler which means no call setup
-  **excessive congestion possible**:  packet delay and loss due to buffer overflow 
-  protocols needed for reliable data transfer, congestion control

**how to provide circuit like behavior with packey switching?**
-  it's complicated and we will study various techniques that will try to make packet switching as circuit like as possible

###  internet structure:  a network of networks

-  hosts connect to internet via access internet service providers (isps)
-  access isps in turn must be inter connected
-  so that any two hosts anywhere can send packets to each other
-  resulting network of networks is very complex
-  evolution driven by economics and national policies

**given millions of access isps, how do we connect them together?**
-  connect each access isp to one global transit isp?
-  customer and provider isps have economic agreement
-  but if one global isp is viable business, there will be competitors...
-  and regional networks may arise to connect access nets to isps

-  at the center there are a small number of well connected large networks
-  tier 1 are commerical isps:  (e.g. sprint, at&t, ntt):  national and international coverage
-  content provider networks (e.g. google, facebook): private network that connects its data centers to internet, often bypassing tier 1, regional isps

###  how do packet delay and loss occur?

-  packets queue in router buffers, waiting for turn for transmission
-  queue length grows when arrival rate to link temporarily exceeds output link capacity
-  packet loss occurs when memory to hold queued packets fills up

###  packet delay four sources

$d_{nodal} = d_{proc} + d_{queue} + d_{trans} + d_{prop}$

1.  $d_{proc}$: nodal processing
-  checks bit errors
-  determine output link
-  typically, < mirosec

2.  $d_{queue}$:  queueing delay 
-  time waiting at output link for transmission
-  depends on congestion level of router

3.  $d_{trans}$: transmission delay
-  L: packet length (bits)
-  R:  link transmission rate (bps)
-  $d_{trans} = L/R$

4.  $d_{prop}$:  propagation delay:
-  d: length of phyical link
-  s: propagation speed (~2x10^8 m/sec)
-  $d_{prop} = d/s$

###  packet queueing delay

-  a:average packet arrival rate
-  L: packet length (bits)
-  R: link bandwidth (bit transmission rate)

$$\frac{L*A}{R}: \frac{\text{arrival rate of bits}}{\text{service rate of bits}}$$

-  $La/R$ ~ $0$:  average queueing delay small
-  $La/R$ -> $1$:  average queueing delay large
-  $La/R$ > $1$:  more work arriving is more than can be serviced - average delay infinite

###  real internet delays and routers

-  what do real internet delays and loss look like?
-  **traceroute program**: provides delay measurement from source to router along end to end internet path towards destination. for all i
-  sends three packets that will reach router i on path towards destination (qith time to live field value i)
-  routers will return packets to sender
-  sender measures time intervals between transmission and reply

###  packet loss

-  queue (aka buffer) preceding link in buffer has finite capacity
-  packet arriving to full queue dropped (aka lost)
-  lost packet may be retransmitted by previous node, by source end system or not at all

###  throughput
-  throughput:  rate (bts/time unit) at which bits are being sent from sender to receiver
-  instantaneous: rate at given point in time
-  average: rate over longer period of time

$R_{s} < R_{c}$:  what is the average end to end throughput?
-  $R_{s}$

$R_{s} > R_{c}$:  what is the average end to end throughput?
-  $R_{c}$

**bottleneck link**
-  link on end to end path that constraints end to end throughput

###  quantitative comparison of packet switching and circuit switching

-  **A circuit switching scenario in which $N_{cs}$ users, each requiring a bandwidth of 20 mbps, must share a link of capacity 200 mbps**
-  **packet switching scenario with $N_{ps}$ users sharing a 200 mbps link, where each user again requires 20 mbps when transmitting, but only needs to transmit 30 percent of the time**

1.  When circuit swqitching is used, what is the maximum number of users that can be supported?

$200/20 = 10$ users

2.  Suppose packet switching is used.  If there are 19 packet switching users, can this many users be supported under circuit switching? Yes or no?

-  each user requires 20 mbps when transmitting
-  each user transmits only 30% of the time
-  there are 19 users sharing a 200 mbps link

-  **average bandwidth per user = $20 \text{ mbps} * 0.3 = 6 \text{ mbps}$**

-  if there are $n_{ps} = 19$ users, then the total average bandwidth requirement is 

-  **total average bandwidth = $19 * 6 \text{mbps} = 114 \text{mbps}$**

therefore the total average bandwidth requirement 114 mbps is less than the link capacity 200 mbps, so 19 packet switching users can be supported

3.  Suppose packet switching is used.  What is the probability that a given specific user is transmitting, and remaining users are not transmitting?

###  network security

-  internet not originally designed with security in mind
-  key things to think about:
-  how bad guys can attack computer networks
-  how we can defend networks against attacks
-  how to design architectures that are immune to attacks

###  packet interception

**packet sniffying**
-  broadcast media (shared ethernet, wireless)
-  promiscuous network interface reads/records all packets (e.g. including passwords) passing by

**ip spoofing**
-  injection of packet with false source address

**denial of sevice (dos)**
-  attackers make resources (server, bandwidth) unavailable to legitimate traffic by overwhelming resource with bogus traffic
1.  select target
2.  break into hosts around the network see botnet
3.  send packets to target from comprised hosts

###  lines of defense

-  authentication: proving you are who you say you are.  cellular networks provides hardware identity via SIM cards, no such hardware assist in traditional internet
-  confidentiality:  via encryption
-  integrity checks:  digital signatures prevent/detect tampering
-  access restrictions:  password-protected vpns
-  firewalls:  specialized middleboxes in access and core networks.  these are off by default and filter incoming packets to restrict senders, receivers, applications.  detecting/reacting to dos attacks
  
###  protocol layers and reference models

**networks are complex with many pieces**
-  hosts
-  routers
-  links of various media
-  applications
-  protocols
-  hardware, software

###  why layering

-  layers:  each layer implements a service, via its own internal layer actions
-  relying on services provided by layer below
-  approach to designing/discussing complex systems
-  explicit structure allows the identification, relationship of system's pieces
-  layered reference model for discussion
-  modularization eases maintenance, updating of system
-  change in layer's service implementation transparent to rest of the system
-  change in gate procedure doesnt affect rest of the system

###  layered internet protocol stack

1.  **application**:  supporting network applications
-  https, imap, smtp, dns

2.  **transport**:  process-process data transfer
-  tcp, udp

3.  **network**: routing of datagrams from source to destination
-  ip, routing protocols

4.  **link**:  data transfer between neighboring network elements
-  ethernet, wifi, ppp

5.  **physical**:  bits on the wire

###  isp/osi reference model

two layers not found in internet protocol stack!

-  **presentation**:  allow applications to interpret meaning of data, e.g. encryption, compression, machine specific conventions

-  **session**:  synchronization, checkpointing, recovery of data exchange

###  services, layering and encapsulation

**Application Layer**

-  Application exchanges messages to implement some application service using services of transport layer.

**Transport Layer**

-  Transport layer protocol transfers M (e.g. reliably) from one process to another, using services of network layer
-  Transport layer protocol encapsulates application layer message, M, with transport layer layer header $H_t$ to create a transport layer segment
-  $H_t$ used by transport layer protocol to implement its service

**network-layer**

-  Network layer protocol transfers transport layer segment $[H_t | M]$ from one host to another using link layer services.
-  network layer protocol encapsulates transport layer segment $[H_t | M]$ with network layer header $H_n$ to create a network layer datagram
-  $H_n$ used by network layer protocol to implement its service.

**link-layer**

-  link layer protocol transfers datagram $H_n | [ H_t | M]$ from host to neighboring host, using network services
-  link layer protocol encapsulates network datagram $[ H_n | [ H_t | M]$, with link layer header $H_I$ to create a link layer frame
