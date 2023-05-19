#### TOP
-----
[AS (Autonomous System Number)](#as%20(autonomous%20system%20number))
[Auto-Negotiation](#auto-negotiation)
[Multicast](#multicast)
[Route-map](#route-map)
[Router-on-a-stick](#router-on-a-stick)
[Port Channel](#port%20channel)
[Unicast](#unicast)

### AS (Autonomous System Number)
----
An Autonomous System (AS) is a collection of interconnected networks under a common administrative domain or control, which share a common routing policy. Each AS is assigned a unique identifier called an AS number, which is used by the Border Gateway Protocol (BGP) to exchange routing information with other routers in the Internet.

AS numbers are divided into two types: 16-bit AS numbers (also known as "old format" AS numbers) and 32-bit AS numbers (also known as "new format" AS numbers). The 16-bit AS numbers range from 1 to 65,535, while the 32-bit AS numbers range from 65,536 to 4,294,967,295.

BGP is the primary protocol that uses AS numbers to exchange routing information between routers in different autonomous systems, and it is commonly used by Internet Service Providers (ISPs) to interconnect their networks. BGP is used to determine the best path to a destination network, based on factors such as the number of hops, the link bandwidth, and the routing policies of the different ASes involved.

In addition to BGP, other routing protocols such as OSPF (Open Shortest Path First) and IS-IS (Intermediate System to Intermediate System) also use AS numbers, but they are used within a single autonomous system to exchange routing information between routers. These protocols do not use AS numbers to exchange routing information between different autonomous systems.

In addition to BGP, EIGRP (Enhanced Interior Gateway Routing Protocol) also uses AS numbers to identify different autonomous systems within a network. However, EIGRP is a distance vector routing protocol used within a single autonomous system to exchange routing information between routers.

Each EIGRP router is configured with a unique 32-bit AS number, which is used to identify the autonomous system. EIGRP routers in the same autonomous system share routing information with each other using a reliable protocol that minimizes network traffic and converges quickly.

Unlike BGP, which is used to exchange routing information between different autonomous systems, EIGRP is used within a single autonomous system to determine the best path to a destination network. EIGRP uses a composite metric that takes into account factors such as bandwidth, delay, reliability, and load to calculate the best path.

[back to top](#top)
### Auto-Negotiation
----
Auto negotiation is a communication protocol used by network devices, such as computers and switches, to automatically configure their network settings, such as data transfer speed, duplex mode (i.e., whether the device can send and receive data simultaneously), and flow control.

When two devices are connected, they send signals to each other to determine the highest speed and best duplex mode they both support. Once they have agreed on the optimal settings, they establish a connection and begin to communicate.

Auto negotiation is an important feature because it eliminates the need for manual configuration of network settings, which can be time-consuming and error-prone. Additionally, it ensures that the devices are using the most efficient settings for their communication, which can improve network performance and reduce errors.

[back to top](#top)
### Multicast
----

[Multicast Vs Unicast](obsidian://open?vault=network%20fundementals&file=Network%20Fundamentals%2FNetwork%20Fundementals%2FExtraContent%2FMulticast%20VS%20unicast)
Multicast is a one-to-many communication model, where data is sent from a single sender to multiple receivers. In this model, the sender sends a message to a multicast IP address, and the message is delivered to all devices that have joined that multicast group. 

Multicast is often used for applications such as video streaming, where multiple users may want to receive the same data simultaneously. Unicast, on the other hand, is used for applications such as web browsing or email, where data is sent to a specific recipient. 

[back to top](#top)
### Route-map
-----

[Example Reference](obsidian://open?vault=network%20fundementals&file=Network%20Fundamentals%2FScenarios%2FSCN_6_LLD)
In networking, a route map is a feature used to manipulate the routing table of a router. It allows you to control how traffic is forwarded through the network by specifying a set of conditions and actions that are applied to incoming or outgoing packets. 

In the example you provided, the route map is being used to filter incoming BGP routes from a specific neighbor (63.2.29.17) using a prefix
list (IPS_IN) that has been previously defined. The "in" keyword specifies that the route map should be applied to incoming BGP updates from the neighbor. 

The route map itself is defined using the "route-map" command, followed by a name and a sequence number. In this case, the name is "ISP_IN" and the sequence number is 10. The "match ip address prefix-list" command is used to specify the prefix list that should be matched against incoming BGP routes. 

Overall, the purpose of this configuration is to permit incoming BGP routes that match the conditions specified in the prefix list, while filtering out routes that do not match.

[back to top](#top)
### Router-on-a-stick
----
Technique to connect a router with a single physical link and perform IP routing between VLANS
- Note Router-on-a-stick is a topology

[back to top](#top)
### Port Channel
----
A port channel, also known as a link aggregation group (LAG) or etherchannel, is a technology used to combine multiple physical network links into a single logical link. This provides increased bandwidth, improved redundancy, and load balancing across the member links. 

Port channels are commonly used in datacenter networks, where a large amount of traffic needs to be handled between switches and servers. By bundling multiple physical links into a port channel, the resulting logical link appears to the network as a single, high-bandwidth link. If any member link in the port channel fails, the traffic is automatically rerouted to the other active links, reducing the risk of downtime. 

Port channeling is supported by many network devices and can be configured in various ways, including static channeling (where the administrator manually configures the set of member links) and dynamic channeling (where the switch automatically negotiates the addition and removal of member links). 

Overall, port-channels are a simple and effective way to increase bandwidth, redundancy, and availability in network infrastructures.

[back to top](#top)
# Unicast
---

[Multicast Vs Unicast](obsidian://open?vault=network%20fundementals&file=Network%20Fundamentals%2FNetwork%20Fundementals%2FExtraContent%2FMulticast%20VS%20unicast)
Unicast is a one-to-one communication model, where data is sent from a single sender to a single receiver. In this model, the sender sends a message to a specific IP address, and the message is delivered only to the device with that IP address. 