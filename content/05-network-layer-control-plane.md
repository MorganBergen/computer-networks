#  computer networks

###  roadmap

-  intra isp routing ospf
-  routing among isp's:  bgp
  
###  making routing scalable

our routing study thus far idealized

-  all routers identical
-  network is flat
-  not true in practice

scale:  bullions of destination
-  can't store all destinations in routing tables
-  routing table exchange would swamp links
  
administrative autonomy
-  internet:  a network of networks
-  each network admin may want to control routing in its own network
  
###  internet approach to scalable routing

aggregate routers into regions known as autonomous systems aka domains

intra-as aka intra-domain: routing among routers within same network
-  all routers in as must run same intra domain protocol
-  routers in different as can run different intra domain routing protocols
-  gateway router:  at edge of its own as, has links to routers in other as's

intra-as aka inter-domain: routing among as's
-  gateways perform inter-domain routing as well as intra-domain routing

###  interconnected autonomous systems

forwarding table configured by intra and inter autonomous system routing algorithms
-  intra-as routing determine entries for destinations within as
-  inter-as & intra-as determine entries for external destinations

###  intra as routing: routing within an as

most common intra-as routing protocols:
-  rip:  routing information protocol
    +  classic distance vector dv:  dvs exchange every 30 seconds
    +  no longer widely used
-  eigrp:  enhanced interior gateway routing protocol
    +  dv based
    +  formerly cisco proprietary for decades became open in 2013
-  ospf:  open shortest path first
    +  link state routing
    +  is-is protocol (iso standard, not rfc standard) essentially same as ospf

###  ospf open shortest path first routing

-  open:  publicly available
-  class link state
    +  each router flood ospf link state advertisements directly over ip rather than using tcp/udp to all routers in enter autonomous system
    +  multiple link costs metrics possible:  bandwidth, delay
    +  each router has full topology, uses dijkstra's algorithm to compute forwarding table
-  security:  al ospf messages authenticated to prevent malicious intrusion

###  hierarchical ospf

-  two level hierarchy:  local area, backbone
    +  link state advertisements flooded only in area, or backbone
    +  each node has detailed area topology; only knows direction to reach other destinations

-  area border routers:  summarizes distances to destinations in own area, advertise in backbone
-  boundary router:  connected to other as's
-  backbone router:  runs ospf limited to backbone
-  local routers:
    +  flood ls in area only
    +  compute routing within area
    +  forward packets to outside via area border router
-  internal routers

###  internet inter-as routing:  bgp

-  bgp (border gateway protocol):  the de facto inter-domain routing protocol
    +  glue that holds the internet together
-  allows subnet to advertise its existence and the destinations it can each to the rest of the internet
-  bgp provides each as a means to
    +  obtain destination network reachability info from neighboring as's ebgp
    +  determine routes to other networks based on reachability information and policy
    +  propagate reachability information to all as-internal routers ibgp
    +  advertise to neighboring networks destination reachability info

###  ebgp, igbp connections

gateway routers run both ebgp and ibgp protocols

###  bgp basics

-  bgp session:  two bgp routers (peers) exchange bgp messages over semi-permanent tcp connections
    +  advertising paths to different destination network prefixes (bgp is a path vector protocol)

###  gbp protocol messages

-  bgp messages exchanged between peers over tcp connection
-  gbp messages
    +  open:  opens tcp connection to remote bgp peer and authenticates sending gbp peer
    +  update:  advertises new path or withdraws old
    +  keepalive:  keeps connection alive in absence of updates; also acks open request
    +  notification:  reports errors in pervious msg; also used to close connection

###  path attributes and bgp routes

-  gbp advertises router: prefix and attributes
    +  as-path:  list of as's through which prefix advertisement has passed
    +  next-hop:  indicates specific internal as router to next hop as
-  policy based routing
    +  gateway receiving router advertisement uses important policy to accept/declae path (e.g. never route through as y)
    +  as policy determines whether to advertise path to other neighboring as

###  bgp path advertisement

###  bgp path advertisement:  multiple paths

###  bgp achieving policy via advertisements

###  bgp populating forwarding tables




