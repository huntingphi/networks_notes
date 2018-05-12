# Networks Chapter 4

## Network layer

  * Transports segment from sending to receiving host
    * On sending side: Encapsulates segments into datagrams
    * On receiving side: Delivers segments to transport layer
  * Network layer protocols are found in ***every*** host and router as all devices are concerned with transportation of segments
  * Router examines header fields in all IP datagrams passing through it
  ![Layers](/home/minad/Pictures/network_layer_0.png)

  #### Two key network layer functions
  * Forwarding: Move packets from router's input to appropriate router output
  * Routing: determine route to be taken by packets from source to destination. *See routing algorithms*

  #### Interplay between routing and forwarding
  ![routing-and-forwarding](/home/minad/Pictures/routing-and-forwarding.png)


... Skip past other sections as not for test 2 ...

  #### NAT: Network Address Translation
  <mark>A way to solve IP Adress shortage?</mark>

  ![NAT](/home/minad/Pictures/NAT.png)
  ![NAT](/home/minad/Pictures/NAT_1.png)

  With NAT we have:
  * 16-bit port number field. This gives the ability to have 60 000 simultaneous connections with a single LAN-side address.
  * NAT is controversial:
    - Routers should only process up to layer 3 <mark>(?but?)</mark>
    - Violates end-to-end argument <mark>(?how?)</mark>
  Address shortage should instead be solved by IPv6

  #### IPv6 Motivation

  Initial motivation: 32-bit addresses were anticipated to be exhausted by 2008
  * <mark>Blocks</mark> were exhausted between 2011 and 2015
  * <mark>Though individual ISP's have some; some recycling</mark>
  Additional motivation:
  * Header format helps speed up processing/forwarding
  * Header changes facilitate QoS <mark>QoS: Quality of Service? How?</mark>

  #### IPv6 datagram format:
    - fixed-length 40 byte header
    - No <mark>fragmentation</mark> allowed
    - 128 bit address space = 3.4 * 10^38 addresses

  ![ipv6-datagram](/home/minad/Pictures/ipv6.png)
  In the above datagram:
  <mark>"ver"</mark>

  "pri" - Priority: Identify priority among datagram in flow

  "flow label": Identify datagrams in same "flow". (Concept of flow is not well defined)

  "next header": Identify upper layer protocol for data

  #### Other changes from IPv4

    * Checksum: Removed entirely to reduce processing time at each hop
    Options: Allowed but outside of header, indicated by "Next header" field
    ICMPv6: New version of <mark>ICMP</mark>
    * Additional message types such as "Packet too big"
    * <mark>Multicast group management functions</mark>

  #### Transition from IPv4 to IPv6

  Not all routers can be upgraded simultaneously.

  How will networks operate with mixed IPv4 and IPv6 routers?

  Tunneling: IPv6 datagram carried as a payload in IPv4 datagram among IPv4 routers

  ![tunneling](/home/minad/Pictures/tunneling.png)

  #### Tunneling

  ![tunneling](/home/minad/Pictures/tunneling_2.png)

  #### Interplay between routing and forwarding
  ![routing-and-forwarding](/home/minad/Pictures/routing-and-forwarding.png)

  #### Graph abstraction

  ![routing-graph](/home/minad/Pictures/routing-graph.png)

  #### Routing algorithm classification

  ##### Global or decentralised information?

  Global:
    * All routers have complete topology, link cost information
    * <mark>"Link state" algorithms</mark>

  Decentralised
    * Router knows physically connected neighbours, link costs to neighbours
    * <mark>"Distance vector" algorithms</mark>

  ##### Staic or dynamic

  Static - Routes change slowly over time

  Dynamic - Routes change more quickly with a periodic update in response to link cost changes

  #### Link-state Routing Algorithm
  ##### Dijkstras algorithm
    * Network topology and link costs are known to all nodes
      - Accomplished via link state broadcast
      - All nodes have the same information
    * Computes lowest cost path from one one node ("Source") to all other nodes. <mark>Gives forwarding table for that node</mark>
    * Iterative: After k iterations, know lowest cost path to k destinations

  ###### Discussion

  Dijkstras algorithm checks all nodes, w, not in N. This is done for each iteration and therefore, for n nodes, does n(n+1)/2 comparisons therefore has O(n^2) complexity. More efficient implementations with O(nlogn) are possible

<mark>Oscillations are possible, where it supports link cost that equals the amount of carried traffic</mark>

![dijkstra_notation](/home/minad/Pictures/dijkstra_notation.png)

![dijkstra_algo](/home/minad/Pictures/dijkstra_algo.png)

![dijkstra_example](/home/minad/Pictures/dijkstra_example.png)

  ### Distance vector algorithm

  #### Bellman-Ford

  Key idea:
  * From time-to-time, each node sends its own distance vector estimate to its neighbours.
  * When node x receives a new DV (DistanceVector) estimate from its neighbour, it updates its own DV using the equation given below. This happens for each node.
  * Under minor natural conditions. the estimate, D<sub>x</sub>(y) converges to the actual least cost d<sub>x</sub>(y)

  ![bellmanford](/home/minad/Pictures/bellmanford.png)
  ![bellmanford_example](/home/minad/Pictures/bellmanford_example.png)
  ![bellmanford_2](/home/minad/Pictures/bellmanford_2.png)

  The algorithm is:

  1. Iterative and asynchronous: Each local iteration is caused by either a local link cost change or a DV update message from neighbour.
  2. Distributed: Each node notifies its neighbours only when its DV changes. Neighbours then notify their neighbours if necessary.

          foreach (node in nodes){
            if(local_link_cost.changed == true && message_from_neighbour){
              recomputeEstimates();
              notify(neighbours)
            }else{
              wait();
            }
          }

  ![bellmanford_table](/home/minad/Pictures/bellmanford_table.png)

  ### Heirarchial routing
    In practice, all destinations cant be stored in routing tables.
    Administrative autonomy - Each network admin may want to control routing in their own networks

    Aggregate routers into regions, known as "autonomous systems"(AS). Routers in same AS run the same routing protocol (intra-AS protocol). Routers in different AS run different intra-routing protocol.

    The gateway router is at the edge of its own AS and links to a router in another AS.

    The forwarding table is configured by both intra- and inter-AS routing algorithm:
    * The intra-AS (protocol) sets entries for internal destinations
    * Inter- and intra-AS sets entries for external destinations

  #### Inter-AS tasks
    If a router in AS 1 receives a datagram destined for somewhere outside of AS 1, the router should forward the packet to a gateway router. How does it know which gateway router to send to?

    AS 1 must learn which destinations are reachable through each neighbouring AS 1 and propagate this information to all the routers in AS 1.

    This is the job of inter-AS routing.

    ##### Example

    Suppose AS1 learns (via the inter-AS protocol) that subnet x is reachable via AS3 (gateway router 1c in the figure below), but not via AS2. The inter-AS protocol then propagates this information to all internal routers

    Router 1d determines from the intra-AS routing information that its interface I is on the least cost path to 1c and then installs forwarding table entry (x,1)

    Now suppose AS1 learns from the inter-AS protocol that subnet x is reachable from AS3 *and* from AS2. To configure the routing table, 1d must determine which gateway it should forward packets to for destination x.

    This is also a job of the inter-AS routing protocol.

    **Hot potato routing**: Send packet towards the closest of the two routers.

    So in conclusion, the intra-AS routing does the following:
      1. Learn from inter-AS that a subnet is available via multiple gateways
      2. Use routing info from intra-AS protocol to determine costs of least-cost path to each of the gateways
      3. Hot potato route
      4. Determine from the forwarding table the interface I that leads to the least-cost gateway. Enter(x,I) into the forwarding table.

  #### Intra-AS routing
    Also known as *interior gateway protocols(IGP)*.

    The most common intra-AS routing protocols:
      * RIP: Routing Information Protocol
      * OSPF: Open Shortest Path First
      * IGRP: Interior Gateway Routing Protocol

  ##### RIP - Routing Information Protocol

  Was included in BSD-Unix distribution in 1982. The routing tables are managed by an application level process called route-d.

   Uses a distance vector algorithm
    * Distance metric: No. hops (max - 15 hops), each link has cost 1
    * DV's exchanged with neighbours every 30s in response msg aka advertisement
    * Each advertisement: List of up to 25 destination subnets

  ![rip](/home/minad/Pictures/rip.png)

  #### OSPF - Open Shortest Path First
    This protocol is open and publicly available.

    It uses a state algorithm:
      * <mark>LS packet dissemination</mark>
      * topology map at each nodes
      * Route computation done using Dijkstras algorithm

  OSPF advertisement carries one entry per neighbour and these advertisements are flooded to the entire AS. They're carried in OSPF directly over IP (as opposed to TCP or UDP)

  ##### Heirarchial OSPF
  2-level hierarchy: <mark>Local area, backbone</mark>
    * Link state advertisements only in area
    * Each node has a detailed area topology; only knowing direction of shortest path to nets in other areas

  Area border routers: Summarize distances to nets in its own area and advertises to other Area Border routers

  Backbone routers: Run OSPF routing limited to backbone

  Boundry routers: Connect to other AS's

  ![ospf](/home/minad/Pictures/ospf.png)

    #### Inter-AS routing

    BGP - Border Gateway Protol is the de facto inter-domain routing protocol. Its the glue that holds the internet together.

    BGP provides AS a means to:
      1. Obtain subnet reachability information from neighbouring AS's - through eBGP sessions
      2. Propogate reachability information to all AS-external routers - through iBGP sessions
      3. Determine good routes to other networks based on reachable information and policy.
    Allows subnet to advertise its existence to the rest of the internet

    ##### BGP basics

    When two BGP routers ("peers") exchange BGP messages its called a BGP session. They advertise paths to different network prefixes (path vector protocol). This is exchanged over semi-permanent TCP connections

    See the figure below - When AS1 advertises a prefix to AS2:
      * AS3 promises it will forward datagrams towards that prefix
      * AS3 can aggregate prefixes in its advertisements

    Using an eBGP session between 3a and 1c. AS3 sends prefix reachability information to AS1.

    1c can then use iBGP to distribute new prefix info to all routers in AS1

    1b can then re-advertise the new reachability to AS2 via eBGP session between 1b and 2a

    When a router learns of a new prefix it creates an entry for the prefix in its own forwarding table

    ![bgp](/home/minad/Pictures/bgp.png)

    ##### BGP routing policy

    In the diagram below:
      * A, B and C are provider Networks
      * X, W and Y are customers of the provider networks
      * X is dual-homed. This means its attached to two networks.
        - X doesn't want to route from B via X to C, and therefore wont advertise to B a route to C. (If it advertises the route it means the route is possible through it therefore would go longer than just going directly)
      * A advertises path AW to B
      * B advertises path BAW to X
      * Should B advertise BAW to C?
        - No as B gets no revenue for routing CBAW since neither W nor C are customers of B
        - B wants to force C to route to W via A
        - B wants to route only to/from its customers

![bgp-routing](/home/minad/Pictures/bgp-routing.png)

    ### Why do we have different Intra-, Inter-AS routing?

    Due to:
      * Policy:
        - Inter-AS: admin who wants to control how its traffic is routed and who routes through its network
        - Intra-AS: single admin so no policy decisions needed
      * Scale: Hierarchal routing saves table size, reduces update traffic
      * Performance: Intra-AS can focus on performance, but intra-AS policy may dominate over performance


  ### Broadcast and multicast routing

  Broadcast protocols are used in layers in practice at both the application and network layers
  For example:
  Gnutella, a p2p architecture, uses application level broadcast to broadcast content queries among Gnutella peers

  <mark>Multicast</mark> examples:
    * Transfer of a software upgrade to selection of users
    * Streaming continuous media
    * Shared data applications ie a whiteboard or teleconferencing app
    * data feeds/rss feeds
    * Interactive gaming

# Final Notes

Understand principles behind network layer services such as service models, forwarding vs routing, how a router works, routing, broadcast and multicast.

Know instantiation and implementation in the internet.
