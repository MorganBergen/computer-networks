#  Computer Networks and the Internet

###  Key terms

1.  Hosts
2.  End Systems
3.  Communication links
4.  Packet switches - include routers and link layer switches
5.  Transmission Rate
6.  Packets
7.  Route / Path

Today’s Internet is arguably the largest engineered system ever created by mankind, with hundreds of millions of connected computers, communication links, and switches; with billions of users who connect via laptops, tablets, and smartphones; and with an array of new Internet-connected “things” including game consoles, sur- veillance systems, watches, eye glasses, thermostats, and cars. Given that the Internet is so large and has so many diverse components and uses, is there any hope of understanding how it works? Are there guiding principles and structure that can provide a foundation for understanding such an amazingly large and complex system? And if so, is it possible that it actually could be both interesting and fun to learn about computer networks? 

This first chapter presents a broad overview of computer networking and the Internet. Our goal here is to paint a broad picture and set the context for the rest of this book, to see the forest through the trees. We’ll cover a lot of ground in this introductory chapter and discuss a lot of the pieces of a computer network, without losing sight of the big picture.
We’ll structure our overview of computer networks in this chapter as follows. After introducing some basic terminology and concepts, we’ll first examine the basic hardware and software components that make up a network. We’ll begin at the net- work’s edge and look at the end systems and network applications running in the network. We’ll then explore the core of a computer network, examining the links and the switches that transport data, as well as the access networks and physical media that connect end systems to the network core.

We’ll examine delay, loss, and throughput of data in a computer network and provide simple quantitative models for end-to-end throughput and delay: models that take into account transmission, propagation, and queuing delays. We’ll then introduce some of the key architectural principles in computer networking, namely, protocol layering and service models. We’ll also learn that computer networks are vulnerable to many different types of attacks; we’ll survey some of these attacks and consider how computer networks can be made more secure.

##  1.1  What is the Internet

In this book, we’ll use the public Internet, a specific computer network, as our prin- cipal vehicle for discussing computer networks and their protocols. But what is the Internet? There are a couple of ways to answer this question. First, we can describe the nuts and bolts of the Internet, that is, the basic hardware and software components that make up the Internet. Second, we can describe the Internet in terms of a network- ing infrastructure that provides services to distributed applications. Let’s begin with the nuts-and-bolts description, using Figure 1.1 to illustrate our discussion.

###  1.1.1  A Nuts-and-Bolds Description

The Internet is a computer network that interconnects billions of computing devices throughout the world. Not too long ago, these computing devices were primarily traditional desktop computers, Linux workstations, and so-called servers that store and transmit information such as Web pages and e-mail messages. Increasingly, however, users connect to the Internet with smartphones and tablets—today, close to half of the world’s population are active mobile Internet users with the percentage expected to increase to 75 percent by 2025 [Statista 2019]. Furthermore, nontraditional Internet “things” such as TVs, gaming consoles, thermostats, home security systems, home appliances, watches, eye glasses, cars, traffic control systems, and more are being connected to the Internet. Indeed, the term computer network is beginning to sound a bit dated, given the many nontraditional devices that are being hooked up to the Internet. In Internet jargon, all of these devices are called hosts or end systems. By some estimates, there were about 18 billion devices connected to the Internet in 2017, and the number will reach 28.5 billion by 2022 [Cisco VNI 2020].

End systems are connected together by a network of communication links and packet switches. We’ll see in Section 1.2 that there are many types of communica- tion links, which are made up of different types of physical media, including coaxial cable, copper wire, optical fiber, and radio spectrum. Different links can transmit data at different rates, with the transmission rate of a link measured in bits/second. When one end system has data to send to another end system, the sending end system segments the data and adds header bytes to each segment. The resulting packages of information, known as packets in the jargon of computer networks, are then sent through the network to the destination end system, where they are reassembled into the original data.

A packet switch takes a packet arriving on one of its incoming communication links and forwards that packet on one of its outgoing communication links. Packet switches come in many shapes and flavors, but the two most prominent types in today’s Internet are routers and link-layer switches. Both types of switches forward packets toward their ultimate destinations. Link-layer switches are typically used in access networks, while routers are typically used in the network core. The sequence of communication links and packet switches traversed by a packet from the send- ing end system to the receiving end system is known as a route or path through the network. Cisco predicts annual global IP traffic will reach nearly five zettabytes (1021 bytes) by 2022 [Cisco VNI 2020].

Packet-switched networks (which transport packets) are in many ways similar to transportation networks of highways, roads, and intersections (which transport vehicles). Consider, for example, a factory that needs to move a large amount of cargo to some destination warehouse located thousands of kilometers away. At the factory, the cargo is segmented and loaded into a fleet of trucks. Each of the trucks then independently travels through the network of highways, roads, and intersections to the destination warehouse. At the destination ware- house, the cargo is unloaded and grouped with the rest of the cargo arriving from the same shipment. Thus, in many ways, packets are analogous to trucks, communication links are analogous to highways and roads, packet switches are analogous to intersections, and end systems are analogous to buildings. Just as a truck takes a path through the transportation network, a packet takes a path through a computer network.

End systems access the Internet through Internet Service Providers (ISPs), including residential ISPs such as local cable or telephone companies; corpo- rate ISPs; university ISPs; ISPs that provide WiFi access in airports, hotels, cof- fee shops, and other public places; and cellular data ISPs, providing mobile access to our smartphones and other devices. Each ISP is in itself a network of packet switches and communication links. ISPs provide a variety of types of network access to the end systems, including residential broadband access such as cable modem or DSL, high-speed local area network access, and mobile wireless access. ISPs also provide Internet access to content providers, connecting servers directly to the Internet. The Internet is all about connecting end systems to each other, so theISPs that provide access to end systems must also be interconnected. These lower- tier ISPs are thus interconnected through national and international upper-tier ISPs and these upper-tier ISPs are connected directly to each other. An upper-tier ISP consists of high-speed routers interconnected with high-speed fiber-optic links. Each ISP network, whether upper-tier or lower-tier, is managed independently, runs the IP protocol (see below), and conforms to certain naming and address conventions. We’ll examine ISPs and their interconnection more closely in Section 1.3.

End systems, packet switches, and other pieces of the Internet run protocols that control the sending and receiving of information within the Internet. The Transmission Control Protocol (TCP) and the Internet Protocol (IP) are two of the most impor- tant protocols in the Internet. The IP protocol specifies the format of the packets that are sent and received among routers and end systems. The Internet’s principal protocols are collectively known as TCP/IP. We’ll begin looking into protocols in this introductory chapter. But that’s just a start—much of this book is concerned with networking protocols!

Given the importance of protocols to the Internet, it’s important that everyone agree on what each and every protocol does, so that people can create systems and products that interoperate. This is where standards come into play. Internet standards are developed by the Internet Engineering Task Force (IETF) [IETF 2020]. The IETF standards documents are called requests for comments (RFCs). RFCs started out as general requests for comments (hence the name) to resolve network and protocol design problems that faced the precursor to the Internet [Allman 2011]. RFCs tend to be quite technical and detailed. They define protocols such as TCP, IP, HTTP (for the Web), and SMTP (for e-mail). There are currently nearly 9000 RFCs. Other bod- ies also specify standards for network components, most notably for network links. The IEEE 802 LAN Standards Committee [IEEE 802 2020], for example, specifies the Ethernet and wireless WiFi standards.

###  1.2.2  A Services Description

The Internet from an entirely different angle is an infrastructure that provides services to applications.  These applications include internet messaging, mapping with real time road traffic information, music streaming movie and television streaming, online social media, video conferencing, multiple person games, and location-based recommendation systems.  The applications are sadi to be distributed applications, since they involve multiple end systems that exchange data with each other.  Internet applications run on end systems, they do not run in the packet switches in the network core.  Packet switches facilitate the exchange of data among end systems, and are not concerned with the application that is the source or sink of data.

One key question comes into play which is how does one program running on one end system instruct the Internet to deliver data to another prorgam running on another end system?  End systems are attached to the Internet provide a **socket interface** that specifies how a program running on one end system asks the Internet infrastructure to deliver data to a specific destination program running on another end system.  A **socket interface** is a set of rules that the sending program must follow so that the Internet can deliver the data to the destination program.  

Thus the Internet has a socket interface that the program sending data must follow to have the Internet deliver the data to the program that will receive the data.  

###  1.1.3  What is a Protocol?

A **protocol** defines the format and the order of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and or receipt of a message or another event.

```
Host <--------------------------------------> Server
Host <---- __TCP connection request__ ------> Server
Host <---- __TCP connect reply__ -----------> Server
Host <---- `GET http://www.person.com` -----> Server
Host <---- `<file>` ------------------------> Server
```

The transmission and receipt of messages, and a set of conventional actions taken when these messages are sent and received, are at the heart of this question and answer protocol.

###  Network Protocols

A network protocol is similar to a human protocol, except that the entities exchang- ing messages and taking actions are hardware or software components of some device.  All activity in the Internet that involves two or more communicating remote entities is governed by a protocol.  

Hardware-implemented protocols in two physically connected computer control the flow of bits on the wire between the two network interface cards.

Congestion-control protocols in end systems control the rate at which packets are transmitted between sender and receiver.

Protocols in routers determine a packet's path from source to destination.

##  1.2  The Network Edge

The computers an dother devices connected to the Internet are often referred to as end systems.  They are referred to as end systems because they sit at the edge of the Internet.  Desktop computers, servers, and mobile devices are end systems.  End Systems are also referred to as **hosts** because they host application programs such as web browser programs, a web server program, and e-mail client program, or an e-mail server program.  __Host = end systems__.  Hosts are sometimes further dvided into two categories: **clients** and **servers**.  **Clients** tend to be desktops, laptops, smartphones.  **Servers** tend to be more powerful machines that store and distribute Web pages, stream video, relay e-mail, and so on.

Most servers from which we receive search results, e-mail, web pages, videos and mobile app content reside in large **data centers**.  

###  1.2.1  Access Networks

The network that physically connects an end system to the first router on a path from the end system to any other distant end system are Access Networks.

Today, the two most prevalent types of broadband residential access are **digital subscriber line** known as DSL and cable.  

-  A residence typically obtains DSL Internet access from the same local telephone company (telco) that provides its wired local phone access.   
-  A customer’s DSL modem uses the existing telephone line exchange data with a digital subscriber line access multiplexer (DSLAM) located in the telco’s local central office (CO). 
-  The home’s DSL modem takes digital data and translates it to high-frequency tones for transmission over telephone wires to the CO; the analog signals from many such houses are translated back into digital format at the DSLAM.

-  High speed downstream channel is in the 50 kHz to 1 MHz band range
-  Medium speed upstream channel is in the 4 kHz to 50 kHz band range
-  Ordinary two way telephone channel is in the 0 to 4 kHz band range

On the customer side, a splitter separates the data and telephone signals arriving to the home and forwards the data signal to the DSL modem. On the telco side, in the CO, the DSLAM separates the data and phone signals and sends the data into the Internet.  The DSL standards define multiple transmission rates, including downstream transmission rates of 24 Mbs and 52 Mbs, and upstream rates of 3.5 Mbps and 16 Mbps.

Because the downstream and upstream rates are dif- ferent, the access is said to be asymmetric.  he DSL provider may purposefully limit a residential rate when tiered service (different rates, available at different prices) are offered. 

**DSL** makes use of the telco's existing local telephone infrastructure, cable internet access makes use of the cable television comany's existing cable television infrastructure.  

###  1.2.2 Physical Media

**Physical Medium**:  Refers to the material substance through whcih eletromagnetic waves or optical pulses travel to transmit data from one system to anoter.

**Transmission Process**:  Buts are sent from the source system through a series of transmitter receiver pairs, using various physical media.

**Guided media** include twisted-pair copper wire, coaxial cable, and fiber opic.

**Twisted-pair copper wire** is the most common and inexpensive guided transmission medium.  It consists of two insulated copper wired twisted together to reduce electrical interference.  And is used in telephone networks and LANs supporting data rates from 10 Mbps to 10 Gbps.  A LANs is a local area network which typically spans a small geographical area, usually a single building or a group of closely located buildings.  It's purpose is to facilitate the sharing of resources such as filesx, printers, and internet connections.

A **Coaxial cable** has two concentric copper conductors and is insulated to achieve high data transmission rates.  It's common in cable TV systems and cable internet access.  It can be used as a shared medium, where multiple systems receive signals from a single cable.

**Fiber optics** uses thin, flexible optical fibers to conduct light pulses, supporting extremely high data rates.  It's preferred for long distances and high speed communication due to low signal attenuations and immunity to electromagnetic interference.  And is used in long-haul networks and internet backbones but limited in short-haul use due to it's high costs of optical devices.

**Unguided media** include terrestrial radio channels and satellite radio channels transmit data through the atmosphere and space.

**Terrestrial radio channels** carry signals without physical wires, it's suitable for penetrating walls and providing mobile connectivity.  The characteristics vary based on the environment and distance.  Terrestrial radio channels are classified into three different groups based on distance:  short distance (e.g. wireless headsets), local area (e.g. wireless local area networks), and wide area (e.g. cellular networks).

**Satellite radio channels** use satellites to link ground stations, operating in different frequency bands.  There are geostationary satellites which orbit 36,000 km above earth, maintain a fixed position relative to the earth, with high propagation delays but are suitable for areas with DSL or cable access.

**Low earth orbit satellites** are closer to earth, rotating around it, requiring multiple satellites for continuous coverage, and are being developed for future internet access.

##  1.3  The Network Core

The networks core is a mesh of packet switches and links that interconnect the internet's end systems.  

###  1.3.1  Packet Switching

In a network application end systems exchange **messages** with each other.  Messages can contain anything the application designer wants.  Message may perform a control function or can contain data such as an e-mail message, a JPEG image, or an MP3 audio file.  In order to send a message from a source end system to a destination end system, the source breaks long messages into smaller chunks of data known as **packets**.  Between source and destination, each packet travels through communication links and **packet switches**.  There are two types of packet switches **routers** and **link-layer switches**.  Packets are transmitted over each communication link at a rate equal to the __full__ transmission rate of the link.  So, if a source end system or a packet switch is sending a packet of **L** bits over a link with transmission rate **R** bits/sec, then the time to transmit the packet is **L/R** seconds.

**Store-and-Forward Transmission**  

Most packet switches use a store and forward transmission at the inputs to the links.  Store and forward transmission means that the packet switch must receive the entire packet before it can begin to transmit the firt bit of the packet onto the outbound link.

Consider a network with two end systems connected by a single router.  The source sends three packets each of size $L$ bits to the destination.  At time 0 the source begins to send packet 1, at time $L/R$ the source has transmitted the entire packet 1, and has been fully received by the router, and at time $2L/R$ the router has transmitted the entire packet 1 to the destination.  

**Single Packet**  The delay for a single packet from source to destination over a single link is 

$$$
Total Delay = 2L / R
$$$

where L is the packet length in bits and R is the transmission rate in bits per second.

**Multiple Packets** for a single packet transmitted over **N**  links with $N - 1$ routers in between the end-to-end delay is 

$$$
d_{end-to-end} = NL/R
$$$

**Queuing Delays and Packet Loss**

Each packet switch has multiple links attached to it.  For each attached link, the packet switch has an **output buffer** also known as an **output queue**, which stores packets that the router is about to send into that link.  The output buffer plays a key role in packet switching.  Packets suffer output buffer queuing delays and these delays are variable and depend on the level of congestion in the network.  Since the amount of buffer space is finite, an arriving packet may find that the buffer is completely full with other packets waiting for transmission.  In this case, **packet loss** will occur - either the arriving packet or one of the already queued packets will be dropped.

**Forwarding tables and routing protocols**

In the internet every end system has an address called an IP address.  When a source end system wants to send a packet to a destination end system, the source includes the destination's IP address in the packet's header.  So when a packet arrives at a router in the network, the router examines a portion of the packet's destination address and forwards the packet to an adjacent router.  Each other has a **forwarding table** that maps destination addresses to that router's outbound links.  When a packet arrives at a rounter, the router examines the address and searches it's forwarding table, using this destination address, to find the appropriate outbound link.  The rounter then directs the packet to this outbound link.

**Routing protocols** are used ot automatically set the forwarding tables.  A routing protocol may determine the shortest path from each router to each destination and use the shortest path results to configure the forwarding tables in the routers.

###  1.3.2  Circuit Switching

There are two fundamental approaches to moving data through a network of links and switches:  **circuit switching** and **packet switching**.  In circuit switched networks, the resources needed along a path (buffers, link transmission rate) to provide for communication between the end systems are reserved for the furation of the communication session between the end systems.  In packet switched networks, these resources are not reserved; a session's messages used the resources on demand and may have to wait (or queue) for access to the communication link.  Traditional telephone networks are examples of circuit switched networks.  If someone wants to send information over a telephone network, the network must establish a connection between the sender and the receiver.  This is a bona fide connection for which the switches on the path between the sender and receiver maintain a connection state for that connection.  This connection is called a circuit.  When the network establishes the circuit, it also reserves a constant transmission rate in the network's links for the duration of the connection.  Since a given transmission rate has been reserved for this sender-to-receiver connection, the sender can transfer the data to the receiver at the __guaranteed__ constant rate.  

**Multiplexing in Circuit-Switched Networks**

A circuit in a link is implemented with either **frequency-division multiplexing**  (**FDM**) or **time-division multiplexing** (**TDM**)

PAGE 58

###  1.3.3  A Network of Networks

##  Delay, Loss, and Throuput in Packet-Switched Networks

###  1.4.1 Overview of Delay in Packet-Switched Networks

###  1.4.2 Queuing Delay and Packet Loss

###  1.4.3 End-toEnd Delay

###  1.4.4 Throughput in Computer Networks

##  1.5  Protocol Layers and Their Service Models

###  1.5.1  Layered Architecture

