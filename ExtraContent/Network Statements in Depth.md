[OSPF](#ospf) | [EIGRP](#eigrp) | [BGP](#bgp)

## Question:
##### What are network statements telling the protocols? what is the information they provide?

### OSPF
---
- In OSPF, the network statement is used to identify the interfaces that belong to the OSPF routing process. The syntax for a network statement in OSPF is as follows:

```c
router ospf <process ID>
 network <network address> <wildcard mask> area <area ID>
```

The process ID is the OSPF process number, the network address is the IP address of the interface, and the wildcard mask is used to specify which bits of the IP address should be considered. The area ID is the OSPF area that the interface belongs to.

CONFIGS FOR THE PICTURE BELOW:
```c
router ospf 20
network 10.2.29.200 0.0.0.3 area 0  
network 10.2.29.68 0.0.0.3 area 1  
network 10.2.29.72 0.0.0.3 area 2
redistribute bgp 129
default-information orignated
```
![[Screenshot (59).png]]

### EIGRP
----
- In EIGRP, the network statement is used to identify the networks that should be advertised to other EIGRP routers. The syntax for a network statement in EIGRP is as follows:

```c
router eigrp <AS number>
 network <network address>
```

The AS number is the EIGRP autonomous system number, and the network address is the IP address of the network that should be advertised to other EIGRP routers. Unlike OSPF and BGP, EIGRP does not require a subnet mask to be specified in the network statement.

CONFIGS FOR THE PICTURE BELOW:
```c
router eigrp 29
network 10.3.29.72 0.0.0.3
passive-interface default
no passive-interface g0/2
redistribute static
redistribute connected
default-metric 10000 10 255 1 1500
```

![[Screenshot (62).png]]


### BGP
---
network statements in BGP is only for adding routes to the BGP routing table to advertise routes we explicitly wish to advertise.

To create a network statement for BGP, you need to specify the network prefix that you want to advertise to your BGP neighbors. The network statement tells BGP which networks to advertise to its neighbors and is used to control the distribution of routing information in the BGP network. The syntax for a network statement in BGP is as follows:

```c
router bgp <AS number>
 network <network prefix>
```

The AS number is the autonomous system number that the BGP router is a part of, and the network prefix is the IP address range that you want to advertise to your BGP neighbors.


```c
// rtrIL
router bgp 129
neighbor 63.2.29.17 remote-as 7922    
network 63.2.29.0 mask 255.255.255.0
neighbor 63.2.29.17 soft-reconfiguration inbound
neighbor 63.2.29.17 route-map ISP_IN in
neighbor 63.2.29.17 route-map ISP_OUT out

// SIC-RTR-1
router bgp 229
neighbor 180.3.29.17 remote-as 7922
network 180.3.29.0 mask 255.255.255.0
neighbor 180.3.29.17 soft-reconfiguration inbound
neighbor 180.3.29.17 route-map ISP_IN in
neighbor 180.3.29.17 route-map ISP_OUT out
```

![[Screenshot (65).png]]