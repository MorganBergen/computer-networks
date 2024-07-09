#  the network layer:  data plane

##  introduction

in this section will see how the network layer can provide its host-to-host communication service.  there is a piece of the network layer in each and every host and router in the network, because of this the network layer protocols are among the most challenging in the protocol stack.  the network layer is decomposed into two parts, the data plane and the control plane.  we will first cover the data plane functions of the network layer - **the per-router functions in the network layer that determine how a datagram arriving on one of a router's input links is forwarded to one of that router's output links**.  we will cover **traditional ip forwarding** -  where forwarding is based on a datagrams destination address and **generalized forwarding** -  where forwarding and other functions may be performed using values in several different fields in the datagram's header.  we will study ipv4 and ipv6 protocols and addressing in detail.  

in contrast to the data plane, the control plane is the network wide **logic that controls how a datagram is routed among routers along an end-to-end path from the source host to the destination host**.  the control plane has routing algorithms and routing protocols such a ospf and bgp which are implemented within a router.  software defined networking sdn explicitly separates the data plane and control plane by implementing these control plane functions as a separate service in a remote controller.

##  overview of network layer

<p align="center">
    <img src="./figures/figure4.1.png" width=600px>
</p>

this figure shows two hosts h1 and h2 and several routers on the path between h1 and h2.  the role of the network layer in these hosts and in the intervening router is to send information from h1 to h2.  the network layer in h1 takes segments from the transport layer in h1, encapsulates each segment into a datagram and then sends the datagram to its nearby router, r1.  at the receiving host h2, the network layer receives teh datagrams from its nearby router r2 and extracts the transport layer segments and delivers the segments up to the transport layer at h2.  

the primary data plane role of each router is to forward datagrams from its input links to its output links.

the primary role of the network control plane is to coordinate these local, per router forwarding actions so that datagrams are transferred end-to-end along paths of routers between source and destination hosts.

###  forwarding and routing:  the data and control plane

the primary role of the network layer is to move packets from the sending host to the receiving hosts, this is done with two different network layer functions forwarding and routing.

-  **forwarding**:  when a packet arrives at a router's input link, the router must move the packet to the appropriate output link.  for example, a packet arriving from host h1 to router r1 must be forwarded to router r2 or the next router on a path to h2.  

-  **routing**:  the network layer must determine the route or path taken by packets as they flow from sender to receiver.  routing algorithms are used to determine the path that packets will take from source to destination, for example the path from h1 to h2.  routing is implemented in the control plane of the network layer.

**forwarding** refers to the router local action of transferring a packet from an input link interface to the appropriate output link interface.  **routing** refers to the network-wide process that determines the end-to-end paths that packets take from source to destination.  forwarding happens at the hardware level while routing happens at the software level.  a key element in every network router is its **forwarding table**.  a router forwards a packet by examining the value of one or more fields in the arriving packets header, and then using these header values to index into its forwarding table.  the value stored in the forwarding table entry for those values indicates the outgoing link interface at that router to which that packet is to be forwarded.  

in the figure below a packet with header field value of 0110 arrives to a router.  the router indexes into its forwarding table and determines that the output link interface for this packet is interface 2.  the router then internally forwards the packet to interface 2.  

<p align="center">
    <img src="./figures/figure4.2.png" width=600px>
</p>

###  control plane:  the traditional approach

in the control plane the routing algorithm determines the contents of the routers forwarding tables.  a routing algorithm runs in each and every router and both forwarding and routing functions are contained within a router.  the routing algorithm computes the values for its forwarding table between one router and another.  this communication is performed by exchanging routing messages containing routing information according to a routing protocol.  

###  control plane:  the software defined networking (sdn) approach

here is an alternative approach in which a physically separate remote controller computes and distributes the forwarding tables to be used by each and every router.  as opposed to each router having a routing component that communicates with the routing component of other routers.  

<p align="center">
    <img src="./figures/figure4.3.png" width=600px>
</p>

a remote controller determines and distributes values in forwarding tables.  the control plane routing functionality is separated from the physical router - the routing device performs forwarding only, while the remote controller computes and distributes forwarding tables.  the remote controller might be implemented in a data center and managed by an isp or third party.  routers and remote controllers communicate by exchanging messages containing forwarding tables and other pieces of routing information.  the controller that computes forwarding tables and interacts with routers is implemented by software and is therefore software defined networking.  

##  network service model

when the transport layer at a sending host transmits a packet into the network meaning that it passes it down to the network layer at the sending host, can the transport layer rely on the network layer to deliver the packet to the destination?  when multiple packets are sent, will they be delivered to the transport layer in the receiving host in the order in which they were sent?  will the amount of time between the sending of two sequential packet transmission be the same as the amount of time between their reception?  these questions are addressed by the network service model.  the network service model defines the characteristics of end-to-end delivery of packets between sending and receiving hosts.

**services provided by the network layer**

-  **guaranteed delivery**:  this service guarantees that a packet sent by a source host will eventually arrive at the destination host.
-  **guaranteed deliver with bounded delay**:  this service guarantees delivery within a specified host to host delay bound.
-  **in order packet delivery**:  this service guarantees that packets arrive at the destination in the order that they were sent.
-  **guaranteed minimal bandwidth**:  this service emulates the behavior of a transmission link of a specified bit rate between sending and receiving hosts.
-  **security**:  this service encrypts all datagrams at the source and decrypts them at the destination, thereby providing confidentiality to all transport layer segments.

the internets network layer provides a single service, known as best effort service.  

in the next section we will cover the data plane component of the network layer.  
-  the internal hardware operations of a router, **including input and output packet processing**, the **router's internal switching mechanism**, and **packet queuing and scheduling**.  
-  **traditional ip forwarding**, in which packets are forwarded to output ports based on their destination ip addresses 
-  **ip addressing**, specifically **ipv4** and **ipv6**.  
-  **generalized forwarding** where packets may be forwarded to output ports based on a larger number of header values.  packets may be blocked or duplicated at the router or may have certain header field values rewritten all under software control.
-  **middleboxes** that can perform functions in addition to forwarding

**packet switch** is to mean a general packet switching device that transfers a packet from input link interface to an output link interface according to values in a packet's header fields.  **link-layer switches** are a type of packet switch that base their forwarding decisions on values in the fields of the link layer frame; switches thus referred to as link layer devices.  other packet switches are called **routers**, which base their forwarding decisions on header field values in the network layer datagram.  routers are network layer devices.

##  what's inside a router

we will focus on the forwarding function - the actual transfer of packets from a router's incoming links to the appropriate outgoing links at that router.  the figure below shows a router's architecture.

<p align="center">
    <img src="./figures/figure4.4.png" width=800px>
</p>

-  **input ports** perform several functions.  it performs the physical layer function of **terminating an incoming physical link at a router**, shown in the left most box of the input port.  an input port also performs **link layer functions needed to interoperate with the link layer at the other side of the incoming link**; this is represented in the middle boxes of the input and output ports.  **a lookup function is also performed at the input port**; this will occur in the right most box of the input port.  it is here that the forwarding table is consulted to determine the router output port to which an arriving packet will be forwarded via the switching fabric.  control packets (packets carrying routing protocol information) are forwarded from an input port to the router processor.  the number of ports supported by a router can range from some to hundreds.  
-  **switching fabric**:  the switching fabric connects the router's input ports to its output ports.  this switching fabric is completely contained within the router - a network inside of a network router.
-  **output ports** stores packets received from the switching fabric and transmits these packets on the outgoing link by performing the necessary link layer and physical layer functions.  
-  **routing processor** performs control plane functions.  in traditional routers - it executes the routing protocols, maintains routing tables and attached link state information, and computes the forwarding table for the router.  in sdn routers - the routing processor is responsible for communicating with the remote controller in order to receive forwarding table entries computed by the remote controller and install these entries in the router's input ports.  the routing processor also performs the network management functions that we will see later on.

###  input port processing and destination based forwarding

the input port's line termination function and link layer processing implement the physical and link layers for that individual input link.  the lookup performed in the input port is central to the router's operation -  the router uses the forwarding table to look up the output port to which an arriving packet will be forwarded via the switching fabric.  the forwarding table is either computed and updated by the routing processor or received from a remote sdn controller.  the forwarding table is copied from the routing processor to the line cards over a separate bus indicated by the dashed line from the routing processor to the input line card.  

a shadow copy at each line card, forwarding decision can be made locally at each input port without invoking the centralized routing processor on a per packet basis and thus avoiding a centralized processing bottleneck.  

<p align="center">
    <img src="./figures/figure4.5.png" width=800px>
</p>

in this case of 32-bit ip addresses, a brute force implementation of the forwarding table would have one entry for every possible destination address.  since there are more than 4 billion possible address, this option is out of the question.

**lets suppose that our router has four links number 0 through 3 and that packets are to be forwarded to the link interfaces as follows:**

**link interfaces**

| destination address range                | link interface |
|:-----------------------------------------|:--------------:|
|`11001000 00010111 00010000 0000000`  ->  | 0              |
| <- `11001000 00010111 00010111 11111111` | 0              |
|`11001000 00010111 00011000 00000000`  -> | 1              |
|<-  `11001000 00010111 00011000 11111111` | 1              |
|`11001000 00010111 00011001 00000000` ->  | 2              |
|<-`11001000 00010111 00011111 11111111`   | 2              |
| `otherwise`                              | 3              |

**forwarding table**

| prefix                      | link interface |
|:----------------------------|:--------------:|
| `11001000 00010111 00010`   | 0              |
| `11001000 00010111 00011000`| 1              |
| `11001000 00010111 00011`   | 2              |
| `otherwise`                 | 3              |

the router matches a prefix of the packet's destination address with the entries in the table; if there's a match, the router forwards the packet to a link associated with the match.  packet with destination address `11001000 00010111 00010110 10100001` will go to link interface 0.  however, when there are multiple matches, the router uses the **longest prefix matching rule**; that is, it finds the longest matching entry in the table and forwards the packet to the link interface associated with the longest prefix match.    

once a packet's output port has been determined via the lookup, the packet can be sent into the switching fabric.  lookup is the most important action in input port processing, but many other actions must be taken 

1.  physical and link layer processing must occur
2.  the packet's version number, checksum, and time-to-live fields must be checked and the latter two fields rewritten
3.  counters used for network management such as the number of ip datagrams received must be updated.

the input port steps of looking up a destination ip address and then sending the packet into the switching fabric to the specified output port is a specific case of a more general match plus action abstraction that is performed in many networked devices, not just routers.  in link layer switches, link layer destination addresses are looked up and several actions may be taken in addition to sending the frame into the switching fabric towards the output port.  

##  switching

the switching fabric is at the heart of a router and through this fabric is where packets are actually switched (forwarded) from an input port to an output port.  switching can be accomplished in a number of ways

-  **switching via memory**:  where switching between input and output ports are done under direct control of the cpu (routing processor).  an input port with an arriving packet first signaled the routing processor via an interrupt.  the packet was then copied from the input port into processor memory.  the routing processor then extracted the destination address from the header, looked up the appropriate output port in the forwarding table and copied the packet to the output ports buffers.  in this scenario two packets cannot be forwarded at the same time.

-  **switching via bus** -  input ports transfer a packet directly to the output port over a shared bus, without intervention by the routing processor.  

-  **switching via an interconnection network** -  one way to overcome the bandwidth limitation of a single, shared bus is to use a sophisticated interconnection network, such as those that have been used in the past to interconnect processor in a multiprocessor computer architecture.  a crossbar switch is non blocking a packet being forwarded to an output port will not be blocked from reaching that output port as long as no other packet from two different input ports are destined to that same output port, then one will have to wait at the input, since only one packet can be sent over any given bus at a time.

<p align="center">
    <img src="./figures/figure4.6.png" width=800px>
</p>



















