#  the network layer:  control plane

the control plane component of the network layer is the network wide logic that controls not only how a datagram is routed along an end to end path from source host to the destination host, but also how network layer components and services are configured and managed.  we will cover traditional routing algorithms for computing the least cost paths in a graph; these algorithms are the basis for two widely deployed internet routing protocols:  ospf and bgp.  ospf is a routing protocol that operates within a single isp network.  bgp is a routing protocol that serves to interconnect all of the networks in the internet;  bgp is thus often referred to as the glue that holds the internet together.  traditionally, control plane routing protocols have been implemented together with data plane forwarding functions, monolithically within a router.  as we learned in the introduction to the data plane, software defined networking snd makes a clear separation between the data and control planes, implementing control plane functions in a separate controller service that is distinct, and remote from the forwarding components of the router it controls.  we will cover snd controllers later in the section.

then we will cover some of the nuts and bolts of managing an ip network:  icmp (the internet control messaging protocol) and snmp (the simple network management protocol)

##  introduction

the forwarding table in the case of destination based forwarding and the flow table in the case of generalized forwarding we the principle elements that linked the network layer's data and control planes.  we learned that these tables specify the the local data plane forwarding behavior of a router.  we saw in the case of generalized forwarding, the actions taken could include not only forwarding a packet to a router's output port, but also dropping a packet, replicating a packet, and or rewriting layer 2, 3, or 4 packet header fields.

in this chapter we will study how those forwarding and flow tables are computed, maintained and installed.  in our introduction to the network layer we learned that there are two possible approaches for doing so.

-  **per router control**  the figure below illustrates the case where a routing algorithm runs in each and every router; both a forwarding and a routing function are contained within each router.  each router has a routing component that communicates with the routing components in other routers to compute the values for its forwarding table.  this per routing control approach has been used in the internet for decades.  the ospf and bgp protocols that we'll study are based on this per router approach to control.

<p>
    <img src="./figures/figure5.1.png">
</p>

-  **logically centralized control**  the figure below illustrates the case in which a logically centralized controller computes and distributes the forwarding tables to be used by each and every router.  as we saw in section 4.4 and 4.5, the generalized match-plus-action abstraction allows the router to perform traditional ip forwarding as well as a rich set of other functions such as load sharing, firewalling, and network address translation that has been previously implemented in separate middleboxes.

<p>
    <img src="./figures/figure5.2.png">
</p>

the controller interacts with a control agent in each of the routers via a well defined protocol to configure and manage that router's flow table.  typically the control agent has minimum functionality; its job is to communicate with the controller, and to do as the controller commands.  unlike the routing algorithms in the first figure the cas do not directly interact with each other nor do they actively take part in computing the forwarding table.  this is a key distinction between per router control and logically centralized control.  

by logically centralized control we mean that the routing control service is accessed as if it were a single central service point, even though the service is likely to be implemented via multiple servers for fault tolerance, and performance scalability reasons.  as we see, sdn adopts this notion of a logically centralized controller - an approach that is finding increased use in production deployments.  google uses sdn to control the routers in its internal b4 global wide area network that inter connects its data centers.  swan, from microsoft research, uses a logically centralized controller to manage routing and forwarding between a wide area network and a data center network.  major isp deployments, including com cast's activecore and deutsche telecom access 4.0 are actively integrating sdn into their networks.  sdn control is central to 4g/5g cellular networking as well.  by the end of next year, 75% of our network functions will be fully virtualized and software controlled.  

##  routing algorithms

in this section we will study **routing algorithms** whose goal is to determine good paths (equivalently, routes), from senders to receivers, through the network of routers.  typically a good path is one that has the least cost.  we will see that in practice, however real world concerns such as policy issues also come into play.  we note that whether the network control plane adopts per router control approach or a logically centralized approach, there must always be a well defined sequence of routers that a packet will cross in traveling from sending to receiving host.  thus the routing algorithms that compute these paths are of fundamental importance, and another candidate for our top 10 list of fundamentally important networking concepts.  

a graph is used to formulate routing problems.  recall that graph g = (n, e) is a set of n nodes and a collection of e edges, where each edge is a pair of nodes from n.  in the context of network layer routing, the nodes in the graph represent routers - the point at which packet forwarding decisions are made - and the edges connecting these nodes represent the physical links between these routers.  such a graph abstraction of a computer network is show in the figure below.  when we study the bgp inter-domain routing protocol, we will see that nodes represent networks, and the edge connecting two such nodes represents direction connectivity (known as peering) between the two networks.  

as shown in the figure below an edge also has a value representing its cost.  typically an edge's cost may reflect the physical length of the corresponding link, the link speed, or the monetary cost associated with a link.  for our purposes, well simply take the edge costs as a given and won't worry about how they are determined.  for any edge (x, y) in E, we denote c(x, y) as the cost of the edge between nodes x and y.  if the pair (x, y) does not below to E, we set c(x, y) = ∞.  also we will only consider undirected graphs in our discussion here, so that edge (x, y) is the same as edge (y, x) and that c(x, y) = c(y, x);  however the algorithms we will study can be easily extended to the case of directed links with a different cost in each direction.  also a node y is said to be a **neighbor** of node x if (x, y) belongs to E.

<p>
    <img src="./figures/figure5.3.png">
</p>

given that costs are assigned to the various edges in the graph abstraction, a natural goal of a routing algorithm is to identify the least costly paths between sources and destinations.  to make this problem more precise, recall that a **path** in a graph G = (N, E) is a sequence of nodes $(x_1, x_2, \cdots, x_p)$ such that each of the pairs $(x_1, x_2), (x_2, x_3), \cdots, (x_{p - 1}, x_p)$ are edges in E.  the cost of a path $(x_1, x_2, \cdots, x_p)$ is simply the sum of all the edge costs along the path, that is, $c(x_1, x_2) + c(x_2, x_3) + \cdots + c(x_{p -1}, x_p)$.  given any two nodes x and y, there are typically paths between the two nodes, with each path having a cost.  one or more of these paths is a **least-cost path**.  the least cost problem is therefore clear:  find a path between the source and destination that has least cost.  in the figure above for example, the least cost path between source node u and destination node w is (u, x, y, w) with a path cost of 3.  node that if all edges in the graph have tha same cost, the least cost path is also the **shortest path** (that is, the path with the smallest number of links between the source and the destination).

one way in which we can classify routing algorithms is according to whether they are centralized or decentralized.

-  a **centralized routing algorithm** computes the least cost path between a source and destination using complete global knowledge about the network.  that is, the algorithm takes the connectivity between all nodes and all link costs as inputs.  this then requires that the algorithm somehow obtain this information before actually performing the calculation.  the calculation itself can be run at one site or could be replicated in teh routing component of each and every router.  the key distinguished feature here, however, is that the algorithm has complete information about connectivity and link costs.  algorithms with global state information are often referred to as **link state algorithms**, since the algorithm must be aware of the cost of each link in the network.

-  in a **decentralized routing algorithm**, the calculation of the least cost path is carried out in an iterative, distributed manner by the routers.  no node has complete information about the costs of all network links.  instead each node begins with only the knowledge of the costs of its own directly attached links.  then, through an iterative process of calculation and exchange of information with its neighboring nodes, a node gradually calculates the least cost path to a destination or a set of destinations.  the decentralized routing algorithm we will study is called a distance vector algorithm, because each node maintains a vector of estimates of the costs (distances) to all other nodes in the network.  such decentralized algorithms, with interactive message exchange between neighboring routers is perhaps more naturally suited to control planes where the routers interact directly with each other.  

a second broad way to classify routing algorithms is according to whether they are static or dynamic.  in **static routing algorithms**, routes change very slowly over time, often as a result of human intervention.  **dynamic routing algorithms** change the routing paths as the network traffic loads or topology change.  a dynamic algorithm can be run either periodically or in direct response to topology or link cost changes.  while dynamic algorithms are more responsive to network changes, they are also more susceptible to problems such as routing loops and route oscillation.

###  the link state routing algorithm

in a link state algorithm, the network topology and all link costs are known, that is, available as input to the ls algorithm.  in practice this is accomplished by having each node broadcast link state packets to all other nodes in the network,  with each link state packet containing the identities and costs of its attached links.  in practice this is often accomplished by a **link state broadcast** algorithm.  the result of the nodes broadcast is that all nodes have an identical and complete view of the network.  each node can then run the ls algorithm and compute the same set of least cost paths as every other node.

the link state routing algorithm we present below is known as dijkstra's algorithm.  a closely related algorithm is prim's algorithm.  dijkstra's algorithm computes the least cost path from one node (the source, which we will refer to as u) to all other nodes in the network.  dijkstra's algorithm is iterative and has the property that after the kth iteration of the algorithm, the least cost paths are known to k destination nodes, and among the least cost paths to all destination nodes, these k paths will have the k smallest costs.  lets define the following notation.

-  $D(v)$:  cost of the least cost path from the source node to destination v as of this iteration of the algorithm.
-  $p(v)$:  previous node (neighbor of v) along the current least cost path from the source to v.
-  $N'$:  subnet of nodes; v is in $N'$ if the least cost path from the source to v is definitively known.

the centralized routing algorithm consists of an initialization step followed by a loop.  the number of times the loop is executed is equal to the number of nodes in the network.  upon termination, the algorithm will have calculated the shortest paths from the source node u to every other node in the network.

**link state algorithm for source node u**

```
initialization:
    N' = {u}
    for all nodes v
        if v is a neighbor of u
            then D(v) = c(u, v)
        else 
            D(v) = ∞
loop
    find w not in N' such that D(w) is a minimum
    add w to N'
    update D(v) for each neighbor v of w and not in N':
        D(v) = min(D(v), D(w) + c(w, v))
    /* new cost to v is either old cost to v or known least cost to w plus cost from w to v */
until N' = N
```

as an example, let's consider the network in nearest figure above and compute the least cost paths from u to all possible destinations.  a tabular summary of the algorithm's computation is shown in the table below, where each line in the table gives the values of the algorithm's variables at the end of the iteration.  let's consider the few first steps in detail.

| step | N'      | D(v), p(v) | D(w), p(w) | D(x), p(x) | D(y), p(y) | D(z), p(z) |
|:----:|:-------:|:----------:|:----------:|:----------:|:----------:|:----------:|
| 0    | u       | 2, u       | 5, u       | 1, u       | ∞          | ∞          |
| 1    | ux      | 2, u       | 4, x       |            | 2, x       | ∞          |
| 2    | uxy     | 2, u       | 3, y       |            |            | 4, y       |
| 3    | uxyv    |            | 3, y       |            |            | 4, y       |
| 4    | uxyvw   |            |            |            |            | 4, y       |
| 5    | uxyvwz  |            |            |            |            |            |

-  in the initialization step, the currently known least cost path from u to its directly attached neighbors v, x, and w are initialized to 2, 1, and 5 respectively.  the costs to y and z are set to ∞ because they are not directly connected to u.
  
-  in the first iteration, we look among those nodes not yet added to the set $N'$ and find that node with the least cost as of the end of the previous iteration.  that node is x, with a cost of 1, and thus x is added to the set $N'$.  line 12 of the ls algorithm is then performed to update $D(v)$ for all node v, yielding the results shown in the second line step 1 in the table.  the cost of the path to v is unchanged.  teh cost of the path to w through node x is found to have a cost of 4.  hence this lower cost path is selected and w's predecessor along the shortest path from u is set to x.  similarly, the cost to y through x is computed to be 2, and the table is updated accordingly.

-  in the second iteration, nodes v and y are found to have the least cost paths and we break the tie arbitrarily and add y to the set $N'$ so that $N'$ now contains u, x, and y.  the cost of the remaining nodes not in $N'$, that is nodes v, w, and z are updated via line 12 of the ls algorithm yielding the results shown in the third row.

-  and so on...
  
when the ls algorithm terminates, we have, for each node its predecessor along the least cost path from the source node.  for each predecessor, we also have its predecessor, and so on in this manner we can can construct the entire path from the source to all destinations.  the forwarding table in a node, say node u, can then be constructed from this information by storing, for each destination, the next hop node on the least cost path from u to the destination.  the figure below shows the resulting least cost paths and forwarding table in u for the network.

<p>
    <img src="./figures/figure5.4.png">
</p>

what is the computational complexity of this algorithm?  that is, given n nodes, not counting the sour node, how much computation must be done in the worst case to find the least cost paths from the source to all destinations>  in the first iteration, we need to search through all n nodes to determine the node, w, not in $N'$ that has the minimum cost; in the third iteration n - 2 nodes, and so on.  overall the total number of nodes we need to search through over all iterations is n(n + 1)/2, and thus we say that the preceding implementation of the ls algorithm has worst case complexity of ordre n squared: $O(n^2)$.

<p>
    <img src="./figures/figure5.5.png">
</p>

the figure above shows a simple network topology where link costs are equal to the load carried on the link, for example reflecting the delay that would be experienced.  in this example, link costs are not symmetric; that is, c(u, v) equals example, node z originates a unit of traffic destined for w, node x also originates a unit of traffic destined for w, and node y injects an amount of traffic equal to w, also destined for w.  the initial routing is shown in figure a with the link costs corresponding to the amount of traffic carried.

when the ls algorithm is next run, node y determines (based on the link costs shown in figure a) that the clockwise path to w has a cost of 1, while the counterclockwise path w has a cost of 1 + e.  hence y's least cost path to w is now clockwise.  similarly, x determines that its new least cost path to w is also clockwise, resulting in costs shown in figure b.  when the ls algorithm is run next, nodes x, y, and z all detect a zero cost path to w in the counterclockwise direction, and all route their traffic to the counterclockwise routes.  the next time the ls algorithm is run, x, y, and z all the route their traffic to the clockwise routes.

what can be done to prevent such oscillations (which can occur in any algorithm, not just an ls algorithm, that uses a congestion or delay based link metric)?  one solution would be to mandate that link costs not depend on the amount of traffic carried - an unacceptable solution since one goal of routing is to avoid highly congested links.  another solution is to ensure that not all routers run the ls algorithm at the same time.  this seems a more reasonable solutions, since we would hope that even if routers ran the ls algorithm with the same periodicity, the execution instance of the algorithm would not be the same at each node.  

###  the distance vector routing algorithm

whereas the ls algorithm is an algorithm using global information, the **distance vector dv** algorithm is iterative, asynchronous, and distributed.  it is distributed in that each node receives some information from one or more of its directly attached neighbors, performs a calculation, and then distributes the results of its calculation back to its neighbors.  it is iterative in that this process continues on until no more information is exchanged between neighbors.  the algorithm is asynchronous in that it does not require all of the nodes to operate in lockstep with each other.  

before we present the dv algorithm, it will provide beneficial to discuss an important relationship that exists among the costs of the least cost paths.  let $d_x(y)$ be the cost of the least cost path from node x to node y.  then the least costs are related by the celebrated bellman-ford equation:

$$d_x(y) = \text{min}_v\{c(x, v) + d_v(y)\}$$

where the $\text{min}_v$ in the equation is taken over all of x's neighbors.  the bellman ford equation is rather intuitive.  indeed, after traveling from x to v, if we then take the least cost path from v to y, the path cost will be $c(x, v) + d_v(y)$.  since we must begin by traveling to some neighbor v, the least cost from x to y is the minimum of $c(x, v) + d_v(y)$ taken over all neighbors v.

let's check for the validity of the equation for source node u to destination node z in the figure below.  the source node u has three neighbors:  node v, x, and w.  by walking along various paths in the graph, it is easy to see that $\text{d}_\text{v}\text{(z)} = 5$, $\text{d}_\text{x}\text{(z)} = 3$, and $\text{d}_\text{w}\text{(z)} = 3$.  plugging these values into the equation along with the costs $\text{c(u, v)} = 2$, $\text

<p>
    <img src="./figures/figure5.3.png">
</p>s

































