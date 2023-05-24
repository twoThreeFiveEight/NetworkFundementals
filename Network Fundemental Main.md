## Sections: 
[Lecture1](#lecture1) -> Intro Lecture OSI
[Lecture2](#lecture2) -> Basic Switching
[Lecture3](#lecture3) -> Basic Layer3 
[Lecture4](#lecture4) -> General Routing
[Lecture5](#lecture5) -> Spanning Tree Protocol 
[Lecture6](#lecture6) -> EIGRP
[Lecture7](#lecture7) -> OSPF
[Lecture8](#lecture8) -> BGP/PAT
[Lecture9](#lecture9) -> Redundancy
[Lecture10](#lecture10) -> Filtering
[Lecture11](#lecture11) -> IPsec
[Side Notes](#side%20notes)



## Interesting To Note
----
#### ROUTERS:
- Some routers a an IP address and MAC address ON EACH INTERFACE
	- Meaning each port reponds to a ARP request differently
	- The MAC on any specific interface is usually the BASE MAC of the device + the port number
		- In otherwords if the MAC is a.a.f.0 and it has 4 ports the other ones would be numbered  a.a.f.1, a.a.f.2, a.a.f.3, a.a.f.4.
- If a router receives a packet with a unknown destination IP in the routing table, the packet is dropped. 
	- This is why we use the defualt route. It is the "***Route That Matches All Routes***" and allows the  router to foward the packet if nothing is found with a "longer match" (remember the "Longest Match Rule") through the default route. 

ROUTING TABLE POPULATION METHODS:
- Directly Connected
	- A router will add routes to the network/subnet that are configured on its interfaces to the routing table knowing that its interface matches a network with in the interface IP.
- Static Routes
	- Routes Manually provided by an administrator
	- can be straight to a subnet or a summerization of many subnets.
```c
ip route <network address> <network subnet mask> <next hop>
```

- Dynamic routes
	- Learned Automatically from other routers.
	- OSPF, EIGRP, BGP, IS-IS, RIP -> common protocols

#### SWITCHES
- Switches cannot do inter VLAN routing. They need to pass the information up to a mls or a router.
	- In other words even if a computer is using the same switch they are technically different networks and need a router to send one packet from vlan 1xx0 to 2xx0. The switch cannot make this happen, There for a switch cannot do inter VLAN routing.

## Lecture1
---- 
Types of Networks 

LAN – Local Area Network – Restricted by a geographic area, 100 to 1,000m 

MAN – Metropolitan Area Network – Public, service provided by a city, 5 to 50km 

WAN – Wide Area Network – Network of networks, internet, up to 100,000km 

Cabling Overview 

Cross-over  

-   Connect like devices; Ex: Router-to-router, switch-to-switch 

Straight-through – Connect unalike devices; Ex: Switch-to-router 

Fiber – Super fragile and made of glass 

-   Single mode - Shorter distances, Large light carrying core, and higher through-put 
    
-   Multi mode – Long distance, Smaller light carrying core, and lower through-put 
    

Stack-wise cables - Redundant power supply, logical switch combination 

Binary Conversion 

Binary – The language of computers, Base 2 

-   Presented as bits – 1's & 0's; "On and Off" 
    
-   8 bits = 1 Byte 
    

Decimal – Base 10 

Hexadecimal – Base 16 

-   Uses a combination of Base 10 and letters A – F 
    

IP Address 

-   32-Bit Number 
    
-   Split into 4 octets 
    
-   Each octet is 8 bits (1 Byte) 
    
-   Dotted decimal Format 
    

Subnet Mask 

-   0-32 bits long 
    
-   Divides an IP address into 2 pieces: Network & Host 
    
-   Defines what network an IP address belongs to 
    
    -   /32 – Smallest subnet mask – 1 address 
        
    -   /31 – 2 addresses 
        
    -   /30 – 4 addresses  
        

Network Address - IP address that represents the entire network 

Reserved 

-   Not useable to assign hosts 
    
-   First IP in the range/Always EVEN  
    

Xxxxx.xxxxx.xxxxx.00000000 = 192.168.1.0 

### Broadcast Address:

- IP address that represents all hosts on a network 
	-   All hosts on the network process data sent to this IP address 
- Reserved – Not useable to assign to hosts 
- IP Address where all host bits are set to 1's 
-   Last IP in the range/Odd   

11000000.10101000.00000001.11111111 = 192.168.1.255 

Max Addressable Hosts Math 

The amount of unique hosts that can exist on a network 

Hosts - Must have a unique IP address 

### Reserved IP Addresses on a subnet
-   Network IP 
-   Broadcast IP



# Lecture2
----
[back to top](#sections)

##### Subject: Basic switching

##### Overview: 
- [Duplexes](#duplexes)
- [Domains](#domains)
	- Collision
	- Broadcast 
- [VLAN](#vlan%20(Virtual%20Local%20Area%20Network))
- [SVI](#svi)
- Switchport modes 
- packets & frames
- Mac Address tables.
- #### CDP, LLDP = Layer 2 network discovery protocols


### Duplexes 
----
(talking about wiring configs)
- Full Duplex (***Bidirectional***):
	- Simultaniously transmits & receive data
	- seperate wires for transmiting and receiving
	- TX -> Transmit
	- RX -> Receive

- Half Duplex:
	- Can only transmit or receive data at one time
	- Share wire for RX and TX. 
 
![[duplexes.png]]


### Domains
----
- #### Collision Domain:
	- Locations where collisions can occur
	- Shared media, single cable, hubs
- #### Broadcast Domain:
	- All the nodes that will receive a ***layer2*** broadcast
	- Logical divisions of a computer network. Same Lan or bridge to other segments.

- #### Domain examples:
	- HUB -> No memory (dumb)
		- Same collision domain
		- Same broadcast domain
		- Received data gets broadcasted out all ports.
	- SWITCH -> Has memory (MAC Address Table)
		- Seperate collision domain
		- Same broadcast domain
		- Each interface (port) has own collision domain
	- ROUTER
		- Seperate collision domain
		- Seperate broadcast domain
		
| DOMAIN | HUB | SWITCH | ROUTER |
|----------|--------|----------|----------- |
| COLLISION | Same | Seperate | Seperate |
| BROADCAST| Same | Same | Seperate |



### VLAN (Virtual Local Area Network)
----
- PURPOSE -> Limit broadcast domains
- Logically created (Created with software)
- Layer2 segmentation of a switch -> Limit broadcast domain.
- Locally ***significant to*** a ***switch***
- Each vlan is its own broadcast domain
- Minor security function
- DO NOT USE VLAN 1 -> Because it is CISCO DEFAULT & is bad practice
- Range 1 - 4096 unique VLANs

#### VLAN Configuration:
----
- For each VLAN you need to create (1 - 4096)
```c
vlan <VLAN_ID>
```
- name 
```c
name <NAME>
```

##### VLAN Verification: 
-----
- To list all configuration VLANs, Access ports, & VLAN status
```c
show vlan brief
```
or
```c
show vlan
```
- To show a specific VLAN
```c
show vlan id <VLAN_ID>
```



### Switch Port Modes (for VLAN)
---
- Only pertains to the Switch
- ONLY 2 OPTIONS:
	- Access
	- Trunk

#### Access:
- Single Vlan -> Part of only one vlan
- Typically connects to hosts/ end devices
	- ACCESS MODE WILL NOT CARRY VLAN TAG
- Associates data to VLAN

#### Trunk:
- Multiple VLANs -> Carry one or more VLANs on the same physical link
- Uses 802.1q encapsulation
	- encapsulates packets using 802.1q protocol with a TAG to identify the VLAN on layer2 
- typically connects to other switches
- connects VLANs between switches
- TYPE OF INTERFACE CONFIGURED ON A SWITCH OPPOSITE A ROUTER'S SUB-INTERFACE. (REMEMBER SUB-INTERFACES ONLY EXIST ON A ROUTER-ON-A-STICK ARCHITECTURE'S)



### Configurations:
---
##### Access port configurations commands:
- For each port to be a access port
```c 
interface <fastEthernet 0/1>
```
```c
switchport mode access
```
```c
switchport access vlan <VLAN_ID>
```



### Access Port Verification commands:
----
To show all interfaces, VLAN and status info
```c 
show ip interface status
```

To show VLAN association & switchport mode
```c 
show interface <fastEthernet 0/1> switchport
```

To show the configuration of single interface
```cpp 
// Method 1:
show run 

// Method 2:
show running-config interface <fastEthernet 0/1>

// Method 3:
show run int g0/0
```



### Trunk port config commands: 
----
For each interface to be trunk

```c
Swtich(config)# interfaace <fastEthernet 0/2>
```
```c
Swtich(config-if)# switchport trunk encapsulation dot1q
```
```c
Swtich(config-if)# switchport mode trunk
```
```c
Swtich(config-if)# switchport trunk allowed vlan <10,20,30-40,50>
```
when doing ranges you can use the commas ( 10,20 ) with no space inbetween or you can you the hyphen ( - ) to seperate the ranges.



### Trunk Port Verification commands:
---
To list all trunk interfaces and the allowed vlans:
```c
show interface trunk
```

To show trunk status and encapsulation type:
```c
show interface <f0/2> switchport
```

To show running configurations of the interface:
```c 
show running-config interface <fastEthernet 0/2>
```



### Switch Virtual Interface ( SVI ):
---
Just an interface to remote connection to the ports:

- Switch Virtual Interface:
	-  Also reffered to as a vlan interface
		- VLANs and SVI are NOT the same
	- Virtual interface on the VLAN
	- Has an ip address
	-  L3 Logical interface
		-  A **switch virtual interface** (**SVI**) represents a logical layer-3 interface on a switch.
	- ##### Remote Management
	Q: On layer 2 what is SVI used for?
	- L2 = Remote Managment
	Q: On layer 3 what is SVI used for?
	- L3 = Default Gateways



#### SVI Configuration:
----
- For each SVI to be created:
- ***VLAN MUST ALREADY BE CONFIGURED***

VLAN Creation  (***NEEDS TO BE FIRST***)
```C
switch(config)# vlan <100>
```

```C
switch(config-vlan)# exit
```

Entering SVI
```C
switch(config)# interface vlan <100>
```
```C
switch(config)# description <name it what you like>
```
```C
switch(config)# ip address <IP_ADDRESS> <SUBET MASK>
```
```C
switch(config)# no shut (turn this on)
```



#### SVI Verification:
----
To show all configured VLANs:
```c
show vlan brief
```

To show all interfaces' IP addresses and status:
```c
show ip interface brief
```

To show IP address, interface counters, and status:
```c
show interface vlan <100>
```

To show the running configuration of the interface:
```c
show running-config interface vlan <100>
```

![[20230412_115611.jpg]]



### Layer 3 Packet (Smaller then Layer2 Frame) (IP):
----
- source IP address
- Destination Address
- TTL : L3 = loop prevention mechanism
- Payload

![[20230412_131156.jpg]]



### Layer 2 Frame (bigger then layer 3 packet)(MAC):
----
- Destination MAC Address
	- FF:FF:FF:FF:FF:FF -> Is the destination MAC for the ARP requests at first. Before it knows the default gateway MAC address.
- Source MAC Address
- Type 
	- VLAN tag (802.1q)
- Payload ( contains all of layer 3 packet)
	-  source IP address
	- Destination Address
	- TTL : L3 = loop prevention mechanism
	- Payload (different data in this payload compared to layer 2 payload)
- Frame Check sequence
	- ensures data is recieved correctly
	
![[20230412_131642.jpg]]



### MAC Address Table (Layer2):
----
- ***Ties physical port to MAC address.***
- Unique Physical identifier
- Updates when frames com into port
- ***Makes intellegent decisions about where traffic is going***

![[20230412_132618.jpg]]

##### Commands :
---
To look at the MAC table
```c
show mac adddress-table
```

To show a specific MAC address in the table:
```c
show mac address-table address <MAC_ADDRESS>
```

To show the MAC table for a specific VLAN:
```c
show mac address-table vlan <VLAN_ID>
```


### Switch Forwarding Decisions:
----
- ***Unicast***
	- 1 to 1
	- private direct message
- ***Broadcast***
	- 1 to all
	- global message to all connected devices on vlan
- Flood
	- 1 to all*
	- special use case like ARP
- multicast
	- 1 to many*


### Device Access:
----
- how we connect to communicate with devices
- Out-of-band
	- Physical connection
	- Console cable
- In-band (Loop for router in layer3, SVI for switch in layer2)
	- Virtual connection
	- SSH/Telnet

### ICMP commands (internet control message protocol)
---

- Tests L3 connectivity between 2 hosts
	- source host sends a ***"ping"***
	- Destination replies with a "ping"

```c
Switch# ping <IP_ADDRESS>
``` 

traceroute DONT FORGET ABOUT THIS ONE, VERY USEFUL
```c 
traceroute <ip_address>                    // (cisco ios)
trace

windows# tracert <ip_address>              // windows
LinuxUser$ traceroute <ip_address>         // linux
```




# Lecture3
----
### SUBJECT: BASIC LAYER 3

[back to top](#sections)
[TEST PREP](obsidian://open?vault=network%20fundementals&file=Leture%203%20quiz%20pre-review)

### TABLES:
---

##### MAC Address Table:
- Binds our L1 (Physical) to our L2 (DataLink)
- Port number to MAC Adress
```c
show mac address-table
```

##### ARP Table
- Binds our L2 (Data-Link) to our L3 (Network)
- MAC adress to IP address
- Each device contains a ARP table

Cisco IOS
```c
show ip arp
```

Windows/ Linux
```sh
arp -a
```


### ARP (adress resolution Protocol):
- Protocol that populates the ARP table 
- FF:FF:FF:FF:FF:FF broadcast MAC-> Is the destination MAC for the ARP requests at first. 
- How a device knows which MAC address to use
	- MAC address are 100% necessary for sending traffic
- ARP cache contains a record of each MAC mapped to each IP
- If MAC address is all F's then it is a broadcast address. 
	- I.E. MAC = FFFF.FFFF.FFFF.FFFF
	- This is like the broadcast IP but on layer2
	- THIS IS THE ADDRESS A ARP REQUEST BROADCAST ON AS ITS DESTINATION MAC ADDRESS TO FIND THE SWITCH BEFORE THE DEFUALT GATEWAY MAC ADDRESS GETS RESOLVED.

![[20230413_101818.jpg]]

![[20230413_101855.jpg]]



### Default Gateway:
---
- Hosts need a default gateway to send traffic outside of thier own subnet
- Gateway must be an IP on the local subnet
- Packets destined for a different network are sent to the gateway
- First Layer3 device on the VLAN network is the gateway. 
- Ip address we utilize to send local traffic off to another network


### Host Configuration:
---
- IP address- host ID
- Subnet mask - defines network
- Default gateway - how to leave the network

syntax: 
```c
ip <10.0.0.4> <255.255.255.248> <10.0.0.1>

// ip <IP> <subnet> <default gateway>
```


### Routing Between Subnets
----
- 3 Methods:
	1. Router connected to 2 switches
	2. Router-on-a-stick
		1. SUB-INTERFACES EXIST ONLY ON ROUTER-ON-A-STICK AND MPLS (LAYER 2 VPN) TOPOLOGIES.
			1. logical interface that allows multiple interfaces with different IP addresses to be tied to a single physical interface.
			2. SUB-INTERFACES ARE USED TO LEVERAGE CONFIGURATION OF MULTIPLE INTERFACES ON A SINGLE PORT (INTERFACE).
	3. inter-VLAN routing using a Layer3 switch/Multilayer switch

Router + 2 switches routing between two subnets
![[20230413_103427.jpg]]



### Sub interfaces/ Router-on-a-stick
---
- Are only used on router-on-a-stick. The other subnet routing methods do not use sub interfaces.
	- Ignore the next sentance if you are in NF -> MPLS which is a layer2 vpn that traverses the internet on predfined paths while never opening up the layer3 packet also uses sub-interfaces on a router.

- Router-on-a-stick configuration.
![[20230413_103444.jpg]]



### Sub interface configuration
---
- Configure the physical interface
```c
interface gigabitEthernet 0/0
no ip address
no shutdown
```

- Configure virtual interfaces
```c
interface gigabitEthernet <0/0.10>
encapsulation dot1q <VLAN_ID>                    // Sub interfaces use 802.1q 
ip address <10.10.0.1> <255.255.255.0>
no shutdown
```

- **Verification commands (router-on-a-stick)

To show configured sub-interface
```c
show ip interfaces brief
```

To show a specific sub-interface:
```c
show ip interface gigabitEthernet <0/0.10>
```

To verifiy new connected interfaces are in the routing table:
```c
show ip route
```



### Layer 3 Switches/Multilayer
---
- Switched that can also route
- Route traffic between VLANs using SVIs
- benifits:
	- Much faster than router-on-a-stick
	- Less delay, no need to reach out for external lines
	- Higher throughput, utilizes "ether" channels & "port" channels
- STILL MAINTAIN ALL L2 SWITCH FUNCTIONALITY

Remember:
- L3 Switch -> SVI = Default gateway
- L2 Switch -> SVI = remote management

- If you see a routing able on a switch it is Layer3
- if DO NOT see a routing table on the switch it is layer 2

![[IMG-20230414-WA0008.jpg]]



#### L3 Switch  Enable /Configuration: 
----
- If the switch is able to do layer3 routing you will need to enable the routing feature:

Enable routing on switch:
```c
ip routing
```

#### ! **NEED TO CONFIGURE VLANS FIRST !
```c
// This needs to be declared before configuring SVI regardless of L2 or L3
vlan <1290>
name management
```

Configure each SVI
```c
interface vlan <10>
```
```c
ip address <10.10.0.1> <255.255.255.0>
```
```c
interface vlan <20>
```
```c
ip address <10.20.0.1> <255.255.255.0>
```



#### L3 Switch Verification:
----

To check if routing is a function:
```c
show ip route
// if nothing pops up it means it is a layer2 switch
```

To check if "ip routing" was configured:
```c
show running-config | <OPTION> <ip-routing>

// OPTIONS: 
// include, exclude, section, begin, end
```



### Loopback Interfaces:
----
- Loopbacks are for remote management
- Logical interface primarily used for managment purposes
- Always in UP/UP state
- Best practice is to use a /32 subnet
	- you want the loopback on its own network so you are using the /32 for the subnet because you eliminate the possibility of anything else on the network. 
		- Remember /32 only has one host on the network.


##### Loop back interface configurations:
---
Create new loopback interface:
```c
interface loopback <0>
```
```c
ip address <10.1.1.1> <255.255.255.255>
// CIDR /32 subnet
```

##### Loopback interface verification:
----

To check interface status:
```c 
show ip interfaces brief
```

To check more details of the interface:
```c
show ip interface <loopback> <0>
```



# Lecture4
-----
[back to top](#sections)

### Subject: General Routing

##### Overview: 
- Routing Table
- Route lookup
- Longest Match Rule
- Static routes
- L3 switch routed interfaces
- Cisco Discovery Protocol/LLDP
- internet Control Message Protocol ICMP
- Route summerization
##### ARP -> all layer3 devices will have a ARP table


### What is routing?
---
- Moving packets - containing data - through a network
	- Process of moving packets through out network
- #### Source & Destination
	- #### routing relies on ***source/destination*** IP's
- Routers route many, many packets per second


### Routing table:
----
- List of next-hop IPs for destination networks
- Lists all the networks a router knows about
	- Route entry for each active interface
	- Route entry for each subnet learned from all protocols
- Types of entries
	- static routes
	- dynamic routes
	- direct connected routes 
- You want to make redundancy
	-  Do not have single points of failure.
- Origin code
	- How the router learned of the route
	- connected, static, OSPF, EIGRP, RIP, BGP, etc.
	- S, O, C, L 
		- S means the IP was learned "statically"
		- C means the IP was learned "connected"
- ##### Administrative Distance (TRUST)
	- Arbitrary number that determines the preference of the route
	- ***LOWER NUMBER IS PREFERED (MORE TRUSTED)
		- This rule is opposite then the Longest match rule


### Administrative Distance 
----
- go [here](#why%20admin%20distance?) for what admin distance is doing

### why admin distance?
----
- when talking about routing protocols there are default administrative distances. Static routing protocol is a defualt of 1 where OSPF default is 110. So we use AD (administrative distance) to be able the configure a different routing protocol. The trusted protocol with AD is the lower value.
- So to change the routing protocol from "static routing" to "OSPF" we increase static routing to any value above 110 (OSPF). In the examples of commands above we the last to last values are changing there administrative distance to select the desired routing protocol

Defaults
- static routing = 1
- ebgp = 20
- eigrp = 90
- OSPF = 110
- ibgp = 200

![[20230414_105217.jpg]]
![[20230414_105751.jpg]]
- #### Router will drop the packet if it cannot find the IP in the table


## Longest Match Rule
----
- Route with the longest/best match will be selected
- Subnet mask of /0 or 0.0.0.0 is the shortest/worst match
- Subnet mask of /32 or 255.255.255.255 is the longest/best match
- Can also be referred to as more or less specific, with the most specific route being selected
	- 0.0.0.0 00000000.00000000.00000000.00000000 -> worst match
	- 10.2.13.0 11111111.11111111.00000000.00000000
	- 10.2.13.0 11111111.11111111.11000000.00000000 -> best match 
		- (longest-match-rule, most specific subnet mask)

- The Longest-match-rule does not concern its self with the number of devices it will have to traverse. It only cares about the most specific route which is is the one with the highest CIDR.


## Static routes: -> WILL BE TESTED ON
----
- Manually add routes to routing table
- ##### ** TEST **
	- A few Cons:
		- Lack of scalability
		- Lack of redundancy
		- Higher chance of human error
		- High administrative overhead
	- A few Pros:
		- Lower CPU usage
		- Mildly more secure
			- less protocols that can be exploited.
##### Hacking tip:
- One can inject IPs into a network that has dynamic routing to redirect there traffic. 


## Static routing Requirements
----
- ##### All 3 requirements must be met for a route to be entered into the routing table
	- The next-hop IP address must be accessible, i.e. in a connected subnet
	- Valid Destination Network address
	- Valid Subnet size
```c
ip route <network address> <subnet mask> <next hop> 
```


### Default route
----
- This is a route that matches all routes
	- notation: 0.0.0.0/0
	- The routing because this subnet is the entire internet. The router will use the routing table to look for the destination IP of the packet. If the subnet for the destination IP is not found by the time it reaches the "default route" it will match the defualt route. 
		- The default route is the gateway to the internet essentially. Without it the router would not know to send the packet out unless there was an exact subnet match.


### Static routing Parameters
---
- 3 Mandatory parameters:
	- Destination network
	- Destination subnet mask
	- Valid Next-hop IP address
		- Destination network address 

- Optional parameters:
	- Name
	- Admin distance
		-  Static route AD = 1
			- If there is a static route inside the routing table you do not wish you make the preferential route you will need to remove it because all other protcols are higher ADs meaning less trusted and will not be selected first.


#### Static routing configurations
----
- Configure static routes
```c
// ip route <destination subnet> <subnet mask> <next hop> <name = description>
// ip route <destination subnet> <subnet mask> <next hop> <100> = AD (look above)
ip route 10.0.0.0 255.0.0.0 10.1.1.1 name 10NetSummary // name needs to be on same line
ip route 0.0.0.0 0.0.0.0 10.1.1.1 name DefaultToCore
ip route 10.1.3.12 255.255.255.255 10.3.4.56 200 // 200 administrative distance
ip route 10.1.3.12 255.255.255.255 10.3.4.57 100 // 100 administrative distance

// always verify configuration saved after entry. 
```


#### Static Routing Verification
----
To show configured static routes:
```c
show running-config | include ip route
```

To show static routes installed in routing table:
```c
show ip route static
```


### L3 Switch Routed interfaces
----
- ##### ***IP routing must be enabled*** to route with a Multi Layer Switch (MLS)
```c
ip routing      
```
- ##### have to explicitly say that the L3 interface is not a switchport via:
```c
no switchport
```

- ##### Multiple SVIs
- Behaves just like a port on a router
- NO sub-interfaces
- Is not own IP address
- Doesn't participate in STP
	- STP = Spanning Tree Protocol -> Look into this!


#### L3 Switch Routed Interface Configurations
---
Convert to routed interface
```c
// These two commands go together
interface g0/1
no switchport
```

Configure the routed interface:
```c
// These commands go together.
ip address 10.10.1.2 255.255.255.0
no shutdown
```


#### L3 Routed interface verification (WILL USE THE MOST)
----
[see more about commands here ](obsidian://open?vault=network%20fundementals&file=Commands)

To show all ports' status and IP address:
```c 
show ip interface brief
```

To show a single port with IP related information:
```c
show ip interface g0/1
```

To show a single port with physical statistics:
```c
show interface g0/1
```


## Cisco Discovery Protocol (CDP)
----
- Cisco proprietary
- Used to help map a network
- Discover information about neighboring devices
	- Hostname 
	- Connected port ID
	- IP adddress
	- Type of device
- Layer2 Protocol
- Directly connected devices
- Cisco devices intercept broadcast
	- Cannot use this on anything other then cisco -> hense the first bullet about being proprietary.


#### CDP Verification:
---
To show a list of all directly connected neighbors:
```c
Show cdp neighbor
```

To show detailed information about neighbors:
```c
show cdp neighbor detail
```



### Link Layer Discovery Protocol (LLDP)
----
- LLDP
	- Open source L2 discovery protocol
	- Must be enabled/running to discover devices

##### LLDP Verification commands:
----
```c
show lldp neighbors
```

```c 
show lldp neighbors detail
```



### Internet Control Message Protocol (ICMP) -> PING
----
- Ping
```c
ping <ip address>
ping <domain name>        // only when using a dns server to resolve the address
```

- trace route 
```c
trace <ip address>
```



### Route Summerization
----
- Route summerization aka supernetting is the process by which you take 2 or more smaller subnets and combine them into one larger subnet
- Done to reduce admin overhead
	- Can black hole unintended traffic
		- Black holing does not give a error
- Having a smaller router table will help speed up our routing process I& use less system resources

#### KEY CONCEPT FOR SUPERNETTING
- You can combine two subnets if they are continuous (in sequential order) this process is summerizing the route. AKA route summerization.
	- if you have two seperate subnets that are continuos like so
		- 192.168.20.0/25 - Host range 0 - 127 (128 hosts) (128 - 2 =126 usable)
		- 192.168.20.128/25 -> Host range 128 - 255 (128 hosts) (128 - 2 =126 usable)
	- The above are two seperate subnets can be combined into one subnet that can contain the range for both subnets. the following is the route summerization we can use as our route summerization
		- 192.168.20.0/24 -> Host range 0 - 255 (256 hosts) (256 - 2 = 254 usable)


# Lecture5
----
#### Subject: Spanning tree Lecture
[back to top](#sections)


### QUIZ NOTE:
-----
What is the command to show all interface status's on any cisco device?

```c
show ip interface brief
```

when a request it will record its MAC address on its table and the port number that associates with the MAC address


## Packet and Frame review
----
- Frames do NOT have TTL (no loop prevention mechanism like with packets) < Can cause broadcast storms.

![[20230417_101159.jpg]]

LOOP CAUSES STORM:
![[20230417_101220.jpg]]


### Spanning tree protocol (STP -> Layer2)
----
- Layer 2 protocol
- purpose: to prevent Layer2 Loops (broadcast storms)
- Method: logically block redundant links
-  STP -> build loop-free logical topologies for our network


#### How does stp work?
----
- First, a root bridge must be elected -> Root bridge = King/Queen of topology
	- **Root bridge** = Swtich with the lowest BID
		- root bridge means it makes all decisions on what ports are open and which ones are blocked. Remember this is all to prevent broadcast storms. That is all.
	- **BID** = Bridge ID; priority + MAC address
		- bid = 32768 + mac
	- **Priority** = arbitrary number to influence root bridge election; 
		- ### Defualt value = 32768
			- If default prioity is selected a older switch (Older switch = one manufactured with a lower mac address number then the newer ones) will take priority before the new switch which would be undesirable because we want the newer more powerful switch to be our root. 
		- Priority value range:
			- 0 -> 61440
			- increments in steps of 4096
		- This number we can influence to make the root what we want.
			- setting the priority to "0" will garuentee that switch becomes our root bridge.


### STP Root Election
----
- To determine the root bridge, ALL switches will send out BPDUs
	- untill each 
- BPDU - Bridge Protocol Data Unit
	- Contains BID
	- ##### BPDU = determine how switches communicate in STP
	- contents:
		- mac address of who is sending it
		- BID
		- root ridge RB
- ##### Once each switch gets the BPDU from all other switches it will then determine which switch is the Root Bridge (RB) based on the switch with the lowest BID. (BID = Priority number + MAC address)
![[20230417_111145 1.jpg]]
Root bridge is the switch on the lower right. -> Lowest BID number.


### STP Port Roles (for determining what ports are blocked or deticated)
----
- Root Port
	- Basically a tie breaking mechanism ( if port path cost ties the other port path cost, it will then compare port BIDs if they are the same it will compared the port ID to determine Root port )
	- ##### Lead to root bridge
		- can be multiple root ports 
		- it is all ports going -> towards root bidge
	- Lowest cost to root
	- One per bridge
	- Comparison order for determining root port:
		- Lowest path cost to root bridge
		- lowest sender BID
		- lowest port ID 
			- {Port priority + port #}
				- port priority default = 128
- Designated Port
	- Non-root active port
- Blocked Port
	- Port blocked to prevent loop
	- redundent port
	- needed where a loop is formed
	- decided based on costs for traffic travel path to root bridge.

![[20230417_104900.jpg]]

![[20230417_110012 (2).jpg]]
 
 
###  STP Port Speed Costs
----
- Faster link is the LOWER link cost.
- standard numeric values 
|SPEED|STP PORT COST|
|---|---|
|10 mbps|100|
|100 mbps|19|
|1 gbps|4|
|10 gbps|2|
|10+gbps|1|

![[20230417_111145.jpg]]


### STP Port States: (ALL PORTS GO THROUGH THESE PHASES)
---
Blocked (20s)  
    - Blocked port, but able to receive BPDUs -> still up for redundancy    - Stable state listening for BPDUs/ does not forward Data  
- Listening (15s)  
    - Blocked, but communicating via BPDUs  
    - Transitional state, listening, and sending BPDUs  
- Learning (15s)  
    - Blocked, but allowing traffic to populate the MAC table  
    - Transitional state, listening/ sending BPDUs, Learns MAC address but does not forward data  
- Forwarding  
    - Normal forwarding port  
    - stable state listening/sending BPDUs + Forwards data.
	
- WE CAN SKIP THE 50 SECONDS THIS PROCESS TAKES BY PROGRAMMING OUR PORTS TO SKIP BLOCKED, LEARNING, AND LISTEN PHASE. - Spanning tree Portfast


### Newer Versions of STP
---
- Rapid Spanning Tree (RSTP)
	- Newer port roles and states
	- combines Listening and Blocking states into one state called "discarding state" (saves 20s)
- Per VLAN Tree (PVST+)
	-  Cisco Proprietary
	- runs seperate instances of STP for each VLAN and can set different ROOT BRIDGES for each VLAN 
		- good when you have reduncy or multiple ways out of the network. you want to have multiple Root bridges to avoid having all the traffic going always out of one all the time.
			- its called load balancing and is like threading in computing where we split the load cost between multiple exits out of the network

MULTI SPANNING TREE 
- RPVST+


### STP Port Extention: (ONLY ON ACCESS PORTS)
---
- portfast is an extention of the STP protocol and not the STP its self.
- "Access" ports only 
- Spanning Tree Portfast
	- Forces port to skip listening and learning states and go directly into forwarding
	- Does not skip blocked state
To configure: Spanning-Tree portfast:
```c
// global port portfast selection on all ports
spanning-tree portfast default

// remove spanning-tree portfast
int g0/0
no spanning-tree portfast

// individual port portfast selection
int g0/0
spanning-tree portfast
```

- BPDU Guard
	- Only used sometimes
	- Used in conjunction with portfast
	- "if you happen to receive a BPDU from this port, go directly into err-disable mode."
		- Err-disable -> logically blocks port
		- To recover from err-disable, "bounce" the port (shut/no shut)

```c
// global configuration of BPDU gaurd
spanning-tree bpduguard default

// individual config of BPDU gaurd
int g0/0 
spanning-tree bpduguard enable
```


# Lecture6
----
#### Subject:  EIGRP
[back to top](#sections)

Glossary: 
- Redistribute -> share IP networks with neighbors


### Dynamic Routing
---
 - Allows network topologies to dynamically adjust to changing network conditions.
- pros:
	- Full redundancy
	- Scalability
		- if we add a new route it will automatically share that route with its neighbors.
	- Easy to manage
- Cons:
	- Less control
	- More complex


### Dynamic Protocols
---
- EIGRP -> Enahnced Interior Gateway Routing Protocol
	- cisco proprietary
- OSPF -> Open Shortest Path First
	- open source
- BGP -> Border Gateway Protocol


### Dynamic routing types
---
### TEST Question:
EIGRP = Distance Vector
OSPF = Link-State
BGP = Path Vector

1. Distance Vector -> EIGRP USES THIS
	- Distance (length) + Vector (direction)
	- ##### Also known as "routing by rumor"
	- A device operating with distance vector protocol is ***unaware of the network topology.***
		- Meaning neighbors tell the device these exist but the current router does actually know it just trust this to be true.
		- These devices only know about its directly connected networks & the remote networks it can reach via its neighbors
2. Link-State -> OSPF uses this.
	- Complete knowledge of network
		- Meaning that a device operating with a link-state protocol will build a full topology of the network. 

### EIGRP
---
- Enhanced interior Gateway Routing Protocol
- #### Distance vector protocol
- utilizes autonomous System numbers (AS numbers, ASNs)
	- AS: Globally unique identifiers, Allows the autonomous system to exchange routing info. 
	- ##### Each instance must have matching the AS numbers. 

- Admin distance = 90
	- more trusted than OSPF, only because it is cicso proprietary.

- ### maintains 3 tables:
	- **NEIGHBOR
		- Populates via "hello message"
		- contains: 
			- IP addresses of ***neighbors*** 
			- Next hop interface
			- hold timer
				- counts up from 0 when "hello" message was sent. If it gets to 15 seconds it will assume that the device it sent the "hello" to is down
			- up timer 
				- how long device has been operating
	- **TOPOLOGY
		- Data base of EIGRP where all routes learned from the neighbor are initially stored before the best route is moved/stored in the routing table.
	- **INTERFACE / ROUTING 
		- Shows all interfaces from all devices participating in EIGRP.

- Each router calculates the best path for each destination based on the information provided by its neighbors. 
	- BEST ROUTE -> Successor 
		- route stored in routing table unless it goes down. Then the redundant route is added instead of best route.
	- REDUNDANT ROUTE -> Feasible Successor
		-  route stored in routing table if best route is down.


## Network Statements
----
[network statements in detail](obsidian://open?vault=network%20fundementals&file=Network%20Statements%20in%20Depth)
- Tells EIGPR 3 things
	- Which interfaces to accept traffic in
		- Tells which interface to accept routing protocol packets (hello messages)
	- Which interfaces to send traffic out 
	- What interface subnets to advertise
- Uses Wildcard logic
	- inverse of subnet mask
	- /24 = 255.255.255.0  = 0.0.0.255
```c
// network <network address> <wild card subnet mask> // NO AREA WITH EIGRP
network 10.2.29.208 0.0.0.255
```

##### TEST-> NETWORK STATEMENTS TELL WHICH INTERFACES CAN SEND AND RECEIVE TRAFFIC


### EIGPR NEIGHBORS
---
- Connected L3 devices that share their routes with each other
- #### Relationship is formed through matching network statement
- Parameters that MUST match:
	- Autonomous system number
	- Subnet mask
	- THESE DEVICES ARE DIRECTLY CONNECTED SO THE "NETWORK STATEMENTS" ARE IDENTICAL BETWEEN EACH OTHER. WE NEED TO JUST FIGURE IT OUT ONCE THEN ADD THAT ONE NETWORK ADDRESS TO BOTH ROUTERS TO MAKE SURE THEY MATCH.
- if parameters do NOT match:
	- No neighbor relationship is formed


#### EIGRP CONFIG
----
Create the EIGRP process:
```c
router eigrp 50  // 50 = AS, ASN number
```

Add interfaces to the EIGRP AS:
```c
network 10.2.2.2 0.0.0.3    // some port
network 10.1.1.1 0.0.0.255  // some other port not on same network above  
```


### PASSIVE INTERFACES
----
- Interfaces you do NOT want to form neighbors on
- If you write a network statement containing the subnet of the passive interface:
	- Interface subnet will still be added to the EIGRP topology table 
	- Interface will NOT send "Hellos"
- You explicitly program the interfaces on layer 3 that connect layer 2 devices  because we do not wish to send hello broadcasts to the layer2 devices.


#### Passive interface configurations
---
SINGLE interface:
```c
router eigrp <XX>
passive-interface g0/1
passive-interface g0/2
```

GLOBAL -> All but selected interfaces:
```c
router eigrp <XX>
passive-interface default
no passive-interface g0/3
```


### EIGRP Default Route Configuration
----
- Default route must already be in the routing table

To redistribute: -> share IPs with neighbors
```c
// ip route <default network address> <subnet> <next hop>
ip route 0.0.0.0 0.0.0.0 10.4.6.1

router eigrp 50

redistribute static       // add configured static routes to topology -> WILL TAKE PRIORITY IF YOU DO NOT CHANGE THE AD (ADIMINISTRATIVE DISTANCE)

redistribute connected    // adds all port configured IPs to topology.
```


### REDISTRIBUTIONS
----
1. REDISTRIBUTION:
	- Advertise routes learned from different sources of the SAME protocol
	- sharing routes between the same protocol

2. MUTUAL REDISTRIBUTION:
	- Advertising routes learned from different sources of a DIFFERENT protocol
	- sharing routes between different protocols (could be multiple protocols)

QUIZ: 
Redistribution -> between same protocol
Mutual Redistribution -> between different protocol

![[mutualRedistribution.jpg]]


#### EIGRP REDISTRIBUTION CONFIGURATIONS
----
- First default metric must be set for redistribution

Create default metrics:
```c
router eigrp <XX>

// DO NOT EVER CHANGE THIS. CONFIGURE IT EXACTLY LIKE THIS
default-metric 10000 10 255 1 1500 
```

Redistribute static routes into EIGRP:
```c
redistribute static
```

Redistribute OSPF 10 into EIGRP:
```c
redistribute ospf 10
```


#### EIGRP REDISTRIBUTION VERIFICATION
----
To show EIGRP neighbor table:
```c
show ip eigrp neighbors
```

To show EIGRP topology table: -> will show all routes in topology 
```c
show ip eigrp topology
```

To show EIGRP routes in routing table: 
- Will only show best routes -> show ip eigrp topology to see all
```c
show ip route eigrp
```

PRO TIP: will use `show ip route eigrp` the most.

To show interfaces matching the network statements:
```c 
show ip eigrp interfaces
```

To show configured routing protocols: -> VERY USEFUL FOR NETWORK DISCOVERY
```c
show ip protocols         // VERY USEFULL FOR NETWORK DISCOVERY
```


# Lecture7
----
### Subject: OSPF
[back to top](#sections)

QUIZ:
- What command could you use to add (convert) all svi interfaces on a particular switch to OSPF
```c
redistributed connected subnets
```

### NOTE
----
- Satic routes have a lower default Admin distance (AD). If you do not pay attention the router will send data down lines with a higher cost instead of the shortest path. 
	- Static routes = 1
	- ospf =110
Need to configure the AD on the statics to be higher then the protocol desired.

[CHEAT SHEET](https://packetlife.net/media/library/10/OSPF.pdf)


### Open Shortest Path
---
- #### Link-state protocol 
	- builds entire image of topology. Does not rely on a rumor from a neighbor.
- OSPF process ID
	- not ASN like EIGRP 
	- locally significant to system / where is ASN are globally unique.
- Admin Distance (AD) = 110 (higher than EIGRP)
- Each router calculates best path for data based on the complete knowledge of the network.
	- uses -> dijkstra's algorithm


### OSPF AREAS
----
- Segment into routing hierarchy
- Limit impact of network changes
- Area 0 (aka "backbone area")
	- Must exist
	- #### All other areas must attach to Area 0


### OSPF Router Types
----
![[20230420_114518.jpg]]
- INTERNAL
	- Has an interface in a single area
	- if there is another interface connected to a different area that router is no longer considered a internal router type.
- BACKBONE
	- Has at least one interface in area 0
- ABR (AREA BORDER ROUTER)
	- Interface in 2 or more areas
- ASBR ("AS" border Router)
	- Device which connects routers from another AS
	- meaning a router that in between two different protocols as well as areas.


### Network Statements
----
- [network statements in detail](obsidian://open?vault=network%20fundementals&file=Network%20Statements%20in%20Depth)
- This is a line of code we need to figure out and add to the interface of the router.
- Tells OSPF 3 things:
	- Which interfaces to accept traffic in
		- Network Statements: Create a subnet range match against interfaces & route entries
			- For each match: 
				- Add interface subnets to ospf database
				- begin transmitting hello messages
				- add router entry to ospf data base
	- Which interfaces to send traffic out
		-  Network Statements: Create a subnet range match against interfaces & route entries
			- For each match: 
				- Add interface subnets to ospf database
				- begin transmitting hello messages
				- add router entry to ospf data base
	- What interface subnets to advertise.
- Uses wildcard logic
	- inverse of subnet mask
	- /24 = 255.255.255.0 = 0.0.0.255
- Example syntax:
```c
// The below is is all done under the `router ospf <XX>` config portion
network 10.1.2.3 0.0.0.255 area 0
```


### OSPF Neighbors (LAYER 3)
---
- Connected **L3 devices** that share their routes with each other 
- Relationship is formed through matching network statements
- Parameters that MUST match:
	- Area ID
	- Subnet mask -> in form of wild card notation.
- If parameters MATCH:
	- Form nieghbor relationship
	- Begin sharing routes
- If parameters do NOT MATCH:
	- No neighbor relationship is formed.

QUIZ -> Asks to have ALL routers communicating over ospf how can we have a summerization to connect all these devices -> THE ANSER WILL BE A LARGE SUMMERIZATION WHAT ENCOMPASSES ALL SUBNETS.

QUIZ -> how to summerize all SVI's? -> REDISTRIBUTE CONNECTED SUBNETS


### OSPF configuration
---
Create OSPF process:
```c
router ospf <locally significant number/arbitrary but should match>
```

Add interfaces to process:
```c
network 10.1.1.1 0.0.0.255 area 0
network 10.2.2.2 0.0.0.3 area 1
network 10.1.2.3 0.0.0.0 area 1
```


### OSPF Database
----
- Collection of all the LSAs for an area (Link state advertisments)
	- Link state advertisements
	- how routers tell the other router about the routes it knows of. 
- Shared via LSUs (Link State Update)
	- Like the car that carries the LSAs to there location. LSA is like the person in the car (LSU)
- For each area:
	- Unique Database
	- Each router has complete copy
	- ABRs have a database for each area


### Passive Interfaces
----
- Interfaces you do NOT want to form neighbors on
- If you write a network statement containing the subnet of the passive interface:
	- Interface subnet will still be added to the OSPF database
	- Interface will NOT send "Hellos"

##### QUIZ -> If You were asked what are some RESTRICTIONS you can put on an interface for OSPF -> 
```c
passive-interface <port #>
```


### Passive-interface Configurations:
----
Single interfaces:
```c
router ospf <XX>
passive-interface <port #>    // not sending hello
passive-interface <port #>    // not sending hello
```

ALL BUT SELECTED PORTS:
```c
router ospf <XX>

// Sets all ports the passive (not sending hello)
passive-interface default

// we are explicitly stating which we do want to send hello messages
no passive-interface <port #>     
```


##### OSPF Default information configuration
----
- Must have default route in routing table
	- Learned via static, BGP, EIGRP, etc.
- Default route injected as Type-5 LSA
- Only needed on one router
- Allows top "root" router to be the only one we program the default static route with and the code below is used to propigate the default route to the other computers that we did not explicitly enter this data into.

Enable default route advertisment
```c
router <XX>
default-information originate    // Shares you default route to other routers.
```


#### Redistrubution configuration
----
- Use "subnets" keyword

Redistribute connected routes into OSPF:
```c
router ospf <XX>
redistribute connected subnets
```

Redistribute static routes into OSPF:
```c
router ospf <XX>
redistribute static subnets
```

Redistribute EIGRP AS 75 into OSPF (75 is just and example number)
```c
router ospf <XX>
redistribute eigrp 75 subnets
```



### LINK STATE ADVERTISEMENT (LSA)
---
intra = inside
inter = between

- LSA
	- Information a router sends & receives about network reachability
	- used to construct a routers link state database.

- Type 1 - router LSA
	- Represents a router and connected interfaces
	- intra-area (sent only inside same area)
	- created by each router and contains information about the routers directly connected routes
- Type 3 - Summary LSA
	- A network between two routers
	- intra-area (sent between two areas)
	- Sent from one area to another and is used to advertise a network in the source.
- Type 5 - External LSA
	- A route imported into the OSPF domain
	- created by ASBR to advertise networks from a different AS (ASN) 


### OSPF Path Selection
---
- Best path calculation
	- Cumulative cost to destination
	- Each link between routers has a specific route
	- Cost Based on interface bandwidth (Similar to STP)
	- Lowest cost wins
		- DIJKSTRA'S ALGORITHM = Ref Bandwidth / link speed
- Tie Breaker
	- Intra-Area
		- Destination is in an area router resides in 
	- Inter Area
		- Destination is not in an area router resides in
	- External
		- Destination is external to OSPF


#### OSPF VERIFICATIONS
----
rib = routing information base

TO show OSPF neighbor status:
```c 
show ip ospf neighbor
```

To show interfaces matching the network statements:
```c 
show ip ospf interfaces brief
```

To show routes known in the OSPF database:
```c 
show ip ospf rib    // rib = routing information base.
```


# Lecture8
----
#### Subject: BGP/PAT
[back to top](#sections)


#### Prefixes = Is a IP address with its associated subnet mask
```c
10.2.2.2 255.255.255.0               // this is technically a prefix
```

#### NOTE: Any interface you wish to access from any outside network must be explicitly labeled as `ip nat inside` in order to trigger the NAT an allow communication.

### BGP
----
- Border gateway protocol
	- Exterior Gateway Protocol
	- Routing protocol of the internet
	- two types of BGP
		- EBGP
			- external BGP
			- most common type.
		- IBGP
			- internal BGP
			- requires a full mesh topology. ( all routers must touch all routers = dificult to scale )


### PAT
---
- Port Address Translation (PAT)
	- Translating IP addesses from Private to Public while minimizing Public IP usage.
	- attaches port numbers to one public IP to make the instance unique. In turn minimizing the number of IPs needed for the entire internet. also allows translation to attach a private IP to the IP & port number for communication.


### eBGP
---
- exterior Gateway Protocol
- primary function of BGP is to exchange layer 3 reachability info with other BGP routers
- Routing protocol of the internet
- TCP Port "***179***"
	- This protocol is where sessions get connected. But is a layer 3 protocol
- "Path Vector" protocol
	- AS hop
	- BGP = path vector unlike eigrp = distance vector or ospf = link-state, 
	- calculates best path by the least amount of HOPS from one autonomous system to another (meaning network to network not router to router). It does not take into acoount the cost of the hops.

![[20230424_103604.jpg]]


### AS (Autonomous System Numbers)
----
- Each group of routers running BGP will utilize an autonomous system number
- Valid AS number range 0 - 65,535
	- 0: Reserved 
	- 1 - 64,495: Public AS numbers
	- 64,496 - 64,511: Reserved to use in documentation
	- 64,512 - 65,534: Private AS numbers
	- 65,535: Reserved
- There are 6 different areas in the world that will run a curtain range of these AS numbers. You can find out the area of the world this AS is being used just by referencing this number.


### BGP Table
---
- List of "***PREFIXES***" known by BGP
- "***Prefixes***" shared to peers
	- Technically, BGP does not announce or share routes. 
		- It actually announces the "***PREFIXES***" for route sharing.
	- "***Prefixes***" are added to the local BGP table via network commands
- Add new routes from routing table
- "***FILTERING***" routes in BGP we utilize BOTH "***prefix-lists***" & "***route-maps***"


### NETWORK STATEMENTS
---
- [network statements in detail](obsidian://open?vault=network%20fundementals&file=Network%20Statements%20in%20Depth)
- ##### Must match an EXACT prefix
- add/inject routes to the BGP table from Routing table
- Doesn't interfere with neighbors like iBGPs
- NO wildcard logic (subnet mask syntax)
- network commands work differently in BGP compared to ospf and eigrp
	- It specifies what network to announce.
##### QUIZ 
- 2 ways to inject the routes  into BGP
	1. Network statements
	2. Redistribution


### ADVERTISE SUMMARY NETWORKS
----
- ##### Exact prefixes must exist in routing table
	- use a null route
		- null route is a "fake" route that we use to add summarized networks to our routing table for BGP to ensure that the exact prefix is in the routing table.
	- NULL route
		- Route to nowhere
		- Packets matching this route will be dropped
		- More specific routes are preferred
		 ```C
		// Next hop gets replaced by null0
		ip route 10.0.0.0 255.0.0.0 null0
		```
		- Used to ensure that exact prefix is in routing table

SIDE NOTE: 
- In order to get the advertised summary onto the BGP table all prefixes and summaries must be inside the routing table. To get our summary into the routing table and not mess up the "Longest match rule" we can add the prefix summary into the routing table by using the `ip route` syntax but then attach the null0 at the end to ensure it will never match inside as a viable route but will still be entered into the BGP table


### BGP NEIGHBORS
---
- Manually configured, Not dynamically discovered
- Same AS number = iBGP
- Different AS number = eBGP 
- Neighbor states
	- idle - Neighbor not responding
	- Active - attempting to connect
	- Connect - TCP session establised -> "port 179"
	- Open Sent - Open message sent
	- Open Confirm = Response Received
	- Established - Adjacency Established


##### BGP NEIGHBOR STATEMENTS
----
- Need one for each neighbor
- Specifies the IP address and AS number of a peer
- Peer must have mirror statement
	- EXAMPLE
	```c
	// Device 1 -> with a AS of 65504? is that right?
	router bgp 65505
	// nieghbor <neighbor IP> remote-as <nieghbor ASN>
	neighbor 10.1.2.3 remote-as 65504

	// NEED TO MAKE A MIRRORING STATEMENT NOW NOTICE 

	// Device 2 -> with a AS of 65505? is that right?
	router bgp 65504
	nieghbor 10.1.2.4 remote-as 65505
	```


### MUTUAL REDISTRIBUTION
----
- Advertise routes learned from different sources on same router
[picture of mutual redistribution]
- When a single router operating with 2 or more dynamic routing protocol redistributes routes between the separate dynamic protocols
```c
router ospf 10
redistribute bgp 50
```


### eBPG vs. iBGP
----
- External BGP (eBGP)  
    - Outside the same AS -> different network to different network  
    - BGP peering between routers in different AS  
    - Admin Distance = 20

- Internal BGP (iBGP)  
    - inside same AS -> working inside the same network  
    - BGP peering between routers in the same AS  
    - Admin Distance = 200  
        - Not trusted nearly as much as eBGP, EIGRP  or OSPF
        - will opt for eigrp or ospf before opting in on iBGP  
    - Full mesh requirements        - each router has a wire going to each router.  
        - full mesh -> "split horizon rule" requires a full mesh


#### BGP Configurations
---
Create BGP process:
```c
router bgp <65501>
```

Add network to BGP table:
```c
network <10.1.2.0> mask <255.255.255.0>
network <10.0.0.0> mask <255.0.0.0>
```

add peers to BGP (NEIGHBOR CONFIGURATIONS)
```c

// IMPORTANT -> the nieghbor router need to have the Mirror of the below config

// These commands are the ones that send out stuff and start the TCP connection on port // 179
neighbor <10.1.2.78> remote-as <65505>
neighbor <10.3.5.2> remote-as <65501>
```

redistribute OSPF into bgp:
```c
redistribute ospf <175>   // redistribute ospf <OSPF PID>
```


#### BGP Verifications
----
To view the BGP table:
```c
show ip bgp
```

### TEST -> ones that are ***GREAT FOR TROUBLESHOOTING***

To list configured peers, their state, number of prefixes recieved: ***GREAT FOR TROUBLESHOOTING***
```c 
show ip bgp summary
```

To soft reset peer and resend route advertisements: ***GREAT FOR TROUBLESHOOTING***
```c
clear ip bgp * soft        // clear all soft
```

***GREAT FOR TROUBLESHOOTING***
```c
show ip bgp neighbors
```

***GREAT FOR TROUBLESHOOTING***
```c
show ip route
```

show 
```c 
show ip bgp neighbor [x.x.x.x] received-routes

// Must have "soft-reconfiguration inbound" set for peer indie router bgp 2XX conf.
// this command stores all unfiltered routes from neighbor in seperate database
neighbor x.x.x.x soft-reconfiguration inbound
```

To view routes being sent to neighbor:
```c
show ip bgp neighbor [x.x.x.x] advertised-routes
```


### NETWORK ADDRESS TRANSLATION (NAT)
----
- NAT allows a host that does not have a valid, registered, globally unique IP address to communicate with other hosts through the internet.
- NAT achieves its goal by using a valid registered IP address to represent the private address to the rest of the internet.

- The NAT function changes the private IP addresses to publicly registered IP addressess inside each IP packet.
![[20230424_130953.jpg]]

- CLEVER NOTE: If two companies have overlapping IPs and one buys the other you can join them using NAT to avoid having to change the entire networks IP addressing to avoid the duplicate IP problem.

### NOTE: Any interface you wish to access from any outside network must be explicitly labeled as `ip nat inside` in order to trigger the NAT an allow communication.


### (Overlaod) PAT
---
- Overloading NAT with "***Port Address Translation***"
- When PAT creates the dynamic mapping, it selects not only an inside global IP address but also a unique port number to use with that address
- The NAT router keeps a NAT table entry for every unique combination of inside local IP address and port, with translation to the inside global address and a unique port number assiciated with the inside global address
- NAT overload can use more than 65,000 port numbers, allowing it to scale well without needing many registered IP addresses-in many cases, needing only one inside global IP address
- ***PAT significantly reduces the number of required registered IP addesses***
```c
// 
show ip nat translations
show ip nat statistics
```

![[20230424_132019.jpg]]


##### (OVERLOAD) PAT CONFIGURATIONS
----
```c
interface GigabitEthernet0/0
ip address 10.1.1.3 255.255.255.0
ip nat inside 

interface Serial0/0/0
ip address 200.1.1.249 255.255.255.252
ip nat outside

ip nat inside source list 1 interface Serial0/0/0 overload

access-list 1 permit 10.1.1.2
access-list 1 permit 10.1.1.1

// here is a invisable Denial. it is like an else statement in coding. If access-list 1 is not there then access-list 2 and if those fail the request is denied.
```
##### NOTE: Any interface you wish to access from any outside network must be explicitly labeled as `ip nat inside` in order to trigger the NAT an allow communication.

Reverse NAT setup

```c
// OUTSIDE is case sensitive and is just a label for the internet.
ip nat pool OUTSIDE 10.2.12.249 10.2.12.254 netmask 255.255.255.248

ip nat outside source list 1 pool OUTSIDE add-route  
```


#### NAT VERIFICATIONS
----
To see current ip address translations
```c
show ip nat translations
```

To see NAT translation stats
```c
show ip nat statistics
```


# Lecture9
----
### SUBJECT: Redundancy
[back to top](#sections)

#### TEST -> where does IP helper go -> In the default gateway 

##### Summary
- Port-channels
- HSRP
- DHCP


### Port Channels (aka Etherchannel)
---
- Purpose 
	- Bundle many physical links into one logical link
		- ##### port-channel bundles physical links into a channel-group to create a single logical link that provides the aggregate bandwidth.
	- Provide link redundancy 
	- Increases bandwidth
		- It does not increase speed
		- speed cannot exceed the capabilities of a single link
- Spanning Tree Protocol (STP) see's port-channel as single logical link
- Max number of bundled ports = 8  (typically)
- Speed cannot exceed max speed of single link
- "**Etherchannel**" aggregate the traffic across all available active ports in the channel. Meaning it sends traffic evenly across the ports rather then sending many packets down only one line of the port-channel
![[20230427_100016 1.jpg]]


### Bundling ports
----
- Manual
	- Mode "on"
	- Forces port into etherchannel (risk of broadcast storms)
	- Statically adding ports
	- ***inferior and should not be done. A loop will be created from the port channel its self.
- Dynamic
	- Link Aggregation Control Protocol (LACP)
		- A protocol for auto-negotiation and maintaining LAG
	- Dynamically negotiates etherchannel
	- LACP mode "active"
		- Actively initiates negotiation
	- LACP mode "passive"
		- Only responds to negotiation


### Port-Channel Configuration Overview
----
- Configure the logical port-channel
- Configure the physical interfaces
	- Bind the interfaces to the port-channel


### Port-Channel CONFIGURATIONS:
----
##### TEST: `channel-group <##(arbitrary)> mode <active/passive>` is the actual command that binds the group to the interface

Set up the local port-channel
```c
// setting up the interface 
interface range <g0/1>
switchport trunk encapsulation dot1q
switchport mode trunk 
switchport trunk allowed vlan <##,##>
// channel-goup <some arbitrary number normally 1> mode <LACP mode -> active/passive>
// This command is what actually binds that group
channel-group <##(arbitrary)> mode <active/passive>

interface port-channel <XX>
switchport trunk encapsulation dot1q
switchport mode trunk 
switchport trunk allowed vlan <##,##>
// channel-goup <some arbitrary number normally 1> mode <LACP mode -> active/passive>
// This command is what actually binds that group
channel-group <##(arbitrary)> mode <active/passive>
```


##### VERIFICATIONS
----
To verify the port-channel status: (JUST CHEKS FOR UP UP STATE)
```c
show ip interface brief
```

To list the state of each individual member: (THIS WILL BE MOST USED)
```c
show etherchannel summary
```



### First Hop Redundancy Protocols
----
- ##### THESE PROTOCOLS ARE ONLY NEEDED WHERE THERE ARE DEFUALT GATEWAYS
- Type of protocol, not a specific protocol
- ###### Provides gateway/next-hop redundancy
- ##### GATEWAY REDUNDANCY
- First hop Redundancy Protocols:
	- HSRP = Hot Standby Routing Protocol -> Cisco
	- VRRP = Virtual Router Redundancy Protcol -> open source
	- GLBP = Gateway Load Balancing Protocol - Cisco
	- VPC = Vitrual Port Channel -> Nexus
	- There are others but are uncommon

NOTE: 
both switches are sending signals to each other every 3 seconds. If the timer runs out and a signal is lost from the master switch the slave will take the position as the master and send out a "***Gratuitous ARP signal***" which updates the MAC address tables with the new port interface and updates the vMAC to send traffic through. Note the Mac address for the switches are made into one virtual MAC called "***vMAC***" So when the switch from master to slave is made the only change to the MAC address table is the port number but the Table maintains the same vMAC
[picture about ralationship between the master switch and the slave]
[difference between HSRP and VRRP (redundancy protocols)]


### Types of First Hop Redundancy Protocols
---
- HSRP


### Hot Standby Router Protocol (HSRP)
----
- Purpose
	- Provide gateway redundancy
	- ***Allow traffic to flow through the redundant route in the event of a device/ link failure
- Cisco propriety
- Doesnt modify the routing table
- VIPS are used in HSRP -> in configs the VIPS is when we are writting the "***Standby Commands***"

- Virtual IP Address (VIP)
	- IP address for HSRP
	- IP that isnt tied to a specific interface rather both redundancy routes via a logical interface.
	- must be in the same subnet as the existing network 
	- Sometimes called Standby IP
![[20230427_102527 (2).jpg]]

- Standby group
	- Logical group of routers participating in a HSRP process
	- 0 - 255
	- Best convention is labeling the standby number the same as the vlan
		- the number doesnt need to be the vlan to work but its good for identifying purposes.
	- standby version 2
		- 0 - 4094
	- ***MUST MATCH ON ALL DEVICES*** -> when you have multiple devices and they are both using the same vlan then what ever standby group # you give the SVI for vlan <1290> on the preempt router you must assign the the corresponding SVI on the NON preempt the same group number. 
		- Best convention is labeling the standby number the same as the vlan
			- the number doesnt need to be the vlan to work but its good for identifying purposes.

- Priority ->TEST -> What determines master and slave relationships
	- ***Higher wins
	- Cisco default 100
		- When making the slave router by best practices you should not use the default number. Make it like 90 or something.
- Preempt
	- Allow a higher priority router to return to active
	- ONLY PUT THIS COMMAND ON THE HIGHER PRIORITY ROUTER -> MASTER ROUTER.
	- if the master (preempt) router goes down and comes back up the `standby <1290> preempt` command allows it to become "active" again. 
- Preempt Delay
	- Timer before preemption occurs
	- You can use this so the switch from slave to master only occurs if the new master router is up and "***Healthy***" for 20-30 minutes. This makes sure that if there is a issue with one routers it doesnt keep switching back and forth unless the router had proved up and healthy for X amount of time.

- Virtual MAC Address (vMAC)
	- Virtual MAC associated with VIP
	- Active HSRP router will reply to ARPs with this


#### HSRP CONFIGURATIONS
----
Switch 1 SVI
```c
// Master L3 switch/router
interface vlan <XX>
ip address <vlan IP> <subnet mask>
standby version 2    // 2 is the most common
standby <X> ip <virtual ip (VIP)>
standby <X> priority <higher the most priority>
standby <X> preempt  // This is our master switch and should have a higher priority

// slave L3 switch/router
interface vlan <XX>
ip address <vlan IP> <subnet mask>
standby version 2    // 2 is the most common
standby <X> ip <virtual ip (VIP)>
standby <X> priority <Needs to be a lower priority>
// NO PREEMPT ON SLAVE
```


#### HSRP Verification:
----
To list all HSRP groups
```c 
// will show "P" if preempt -> should only be on higher priority
show standby brief   // easy to read summary

show standby         // much more detailed output
```

To list HSRP group configured for an interface
```c
show standby vlan <vlan ID>
```

![[20230427_104931.jpg]]


### Dynamic Host Configuration Protocol (DHCP)
----
- Purpose:
	- Dynamically assign an IP address, default gateway, and DNS server to hosts instead of having to do this manually for each host.
	- Easy IP address management at scale
- Cons
	- You can forget to exclude some addresses and will be assigning IPs to devices we want to remain static.
	- may run out of IP address to give if we have long of a lease.
		- example: a smaller subnet that is in a area that is transity like guest wifi. IF we set the timer to 5 days and the subnet gets filled up by a bunch of users being in that area then when they leave they leave with that ip so if a new client shows up they may not e assigned a new IP because they are all taken until the LEASE runs out.

- ##### DHCP Does not give out IP addresses it "***LEASES***" them. There is a timer for how long a IP is given to a end point. The timer is up to the engineers discression. 


### DHCP Participants
---
- Client
	- Device requesting IP and gateway
- Relay Agent
	- Device that helps facilitate communication between client and server
	- The "middle man"
- Server
	- Device supplying IP and gateway.
		- cantains pool
- IP HELPER is only put on defualt gateways  that would require dynamic assigned hosts
	- defualt gateway that would require dynamic assigned hosts
	- If static then we wouldnt need to put ip helper on default gateway

NOTE: If we have a loopback on a router using DHCP we do not want it to participate in DHCP so we will have to explicitly EXCLUDE that interface from DHCP. This is ONLY the case if the IP might fall under the subnet of the POOL. 
EX: A pool of 10.0.0.0 255.255.255.0 with a loopback of 10.2.2.2 255.255.255.255 will not need to be EXCLUDED because it will not ever be "black holed into pool"


### DHCP Components
----
- DHCP Service
	- Enables DHCP on a router ("service DHCP")
- DHCP pool
	- Specifies a pool of IP addresses
	- Seperate pool for each subnet
- DHCP Excludes 
	- Addresses in pools not to e given out
- IP Helper Address
	- Enable the DHCP relay on an interface

![[20230427_111619 1.jpg]]

WE WILL NOT "EXCLUDE" THE LOOP BACK BECAUSE IT DOES NOT FALL INTO THE DHCP POOL IN THE ABOVE EXAMPLE


### Request process (DORA)
----
https://www.youtube.com/watch?v=b_9Dg0QYJUg
1. discover
2. offer
3. request
4. acknowledge


#### DHCP Configurations:
---
Enable the DHCP service:
```c
service dhcp
```

Create DHCP pools:
```c
// ip dhcp pool <naming convention. This is just a name>
ip dhcp pool <VLAN_ID_10.100.0.0/24_POOL>
network <10.100.0.0> mask <255.255.255.0>
default-router <10.100.0.1>    // router that will have these pools on it.
```

Exclude IP addresses from DHCP pools:
```c
 // Excludes these addresses from being assigned elsewhere
ip dhcp exclude-address <10.100.0.1>  
ip dhcp exclude-address <10.100.0.2>
ip dhcp exclude-address <10.100.0.7>
```

IP helper Address Config
```c
ip helper-address <10.100.0.1>
```


##### DHCP VERIFICATIONS:
----
To list current DHCP leased addresses:
```c
show ip dhcp binding
```

To list configured DHCP pools:
```c
show ip dhcp pool
```


# Lecture10
----
### Subject: Filtering
[back to top](#sections)
[Prefix list in detail](obsidian://open?vault=network%20fundementals&file=Prefix%20List%20in%20Detail)

##### TOPICS:
- Prefix Lists


### GBP route Filtering
-----
- Control route advertisements
- Filters are applied to neighbors
- Filters may be used by multiple peers
- By default BGP will advertise (propagate) all Prefixes it knows to all neighboring routers.
	- This will crash the internet and crash our selves because there may be a tremendious amount of prefixes that get sent and need to get processed
	- Imagine a ISP sending its entire list to a small companies router. That router could not handle how large that list is.
- Methods:
	- Prefix Lists
		- Prefix = Network IP + Subnet Mask
	- Route-maps
		- Remember - "***FILTERING***" routes in BGP we utilize BOTH "***prefix-lists***" & "***route-maps***"


### Prefix List & Route Maps
----
- Prefix Lists
	- Match exact prefixes
		- If we have a network that does not match exactly to the prefix list we added to the route map that we want to advertise then we will have an issue because it will pass the filter and end up being implicitly denied. So we need to Match that network and Subnet EXACTLY as it shows up to avoid this issue.
- Route Maps 
	- Match prefix lists
	- Match other route attributes
		- EX: metric, next-hop, route-source, route-type, etc.


### Direction of Filtering
---
- #### Relative to local router
- Inbound
	- Filtering advertisements received from peer (Computer we are isolating) 
	- Filtered routes not added to BGP table
- OutBound
	- Filtering advertisements sent to a peer  (Computer we are isolating)
	- BGP table gets filtered 


### Soft-Reconfiguration Inbound
---
- Buffer to store complete advertisement from peer
	- Basically stores a copy of the data flowing through, so we can compare what went through the filter compared to how the ilter actually filtered
- All routes received from peer before filtering
- Enable soft reconfiguration:
```c 
router bgp 65501
neighbor 10.1.1.1 soft-reconfiguration inbound
```
Show routes from peer before filtering:
```c
show ip bgp neighbor 10.1.1.1 received-routes
```


### Prefix Lists
----
- Similar to Named ACLs
- Matches Prefixes, not IP addresses
- #### Implicit deny any -> AT THE END OF EVERY PREFIX LIST (TEST)


### Prefix List Configuration
----
- Create the prefix list:
```c
// always add a sequence number and ALLOWS GIVE YOUR SELF SPACE INBETWEEN SEQ #
ip prefix-list MyPrefixList seq 10 deny 10.2.31.0/24
ip prefix-list MyPrefixList seq 20 permit 10.0.0.0/8
ip prefix-list MyPreFixList seq 30 permit 0.0.0.0/0
// IMPLICIT DENY LIVES HERE
```
- Filter a neighbor's advertisements with Prefix List:
```c
// nieghbor <IP to neighbor you want the rules applied> prefix-list <ListName> in/out
router bgp 6501
neighbor 10.37.51.66 prefix-list MyPreFixList in
```


### Prefix List Verification
---
- To list all prefix lists:
```c
show ip prefix-list
```
- To show a specific prefix list:
```c 
show ip prefix-list MyPrefixList   //  <- MyPreFixeList is the name of list 
```
- To show configured prefix lists:
```c
show running-config | section ip prefix-list
```


### Route Maps
----
- Nested structure
	- Route map entry
	- Match statement
- Cna match a prefix list and/or various route attributes
- ##### Implicit deny any at the end (TEST)
- What are they really?
	- "If-then" programming solution of cisco. 
	- Allows you to check for certain match conditions and optionally set values
- Example of what we can do:
	- only advertise EIGRP routes to neighbor change the next hop address


### Route Map Configurations
----
##### Create a Prefix list to match

Create a route map to filter based on prefix list:
```c
// Creates route map and add route-map sequence numbers of route-map
route-map MyRouteMap1 permit 10
// Now we are inside route map and setting the condition "MATCH" IPs in MyPrefixList
// <condition> ip address prefix-list <ListName>
match ip address prefix-list MyPrefixList 
```

Create additional route map entry:
```c
route-map MyRouteMap2 deny 10
match ip address prefix-list MyPrefixList
route-map MyRouteMap2 permit 20
```

Create route map that matches multiple prefix lists:
```c
route-map MyRouteMap3 permit 10
match ip address prefix-list MyPrefixList1 List2
```

##### Applying filter to BGP

Filter advertisement TO/FROM NEIGHBOR:
```c
router bgp 65501
neighbor 10.1.1.2 route-map MyRouteMap1 out
nieghbor 10.1.1.2 route-map MyRouteMap2 in
```


### Route Map Verification:
---
To show all route maps
```c
show route-map
```

To show a specific route map:
```c
show route-map MyRouteMap1
```


### BGP Route Filtering Verification
----
- ##### NEED TO HAVE SET SOFT-CONFIGURATION PRIOR TO THESE COMMANDS

To show routes from a peer BEFORE filters:
```c
show ip bgp neighbor 10.1.1.2 received-routes
```

To show a route from a peer AFTER filters:
```c
show ip bgp neighbor 10.1.1.2 routes
```

To show routes being sent to a peer after filtering:
```c
show ip bgp neighbor 10.1.1.2 advertised-routes
```


### Access Control Lists (ACLs)
---
- Generic list of permit and deny statemetns
- Used as basic security for networks
- identify interesting traffic for another process
	- what can we use them for
		- Filtering incoming packets 
		- Filtering outgoing packets
		- Resistrict content of routing updates
		- Control VTY access -> remote access control
- Access lists perform packet filtering to control which packets move through the network and where


### ACL Types
---
- Standard ACL
	- ACL number 1 - 99 and 1300 - 1999
	- ##### Matches only the source IP address (TEST)
	- "should" be placed near the destination of the packets.
- Extended ACL
	- ACL numbers 100 - 199 and 2000 - 2699
	- Matches Protocol (IP, TCP, UDP,etc. ), SRC/DST IP, SRC/DST L4 Port Number.
		- The IP protocol type
		- The source IP address
		- The destination 


### ACL Processing
----
- ###### Each line is processed sequentially; top-down
- ##### if a line matches, stop processing
- ##### Implicit "deny any" at the end of all ACLS (TEST)


### Configuring ACLs
----
- First ACL entry creates the ACL Subsequent lines added to the end of the list
- Deleting the ACL will delete the entire list of entries <- IMPORTANT
- To re-order the list, delete lines by sequence numbers and reapply with new sequence numbers


### ACL Traffic Filtering
---
- Applied to an interface
	- One ACL per interface, per direction
- Inbound
	- Filters traffic received into an interface
- Outbound
	- Filters traffic before transmitting out of an interface


### ACL Configuration
---
##### Standard ACL type Config
To define a standard ACL:
```c
access-list <10> permit <10.0.0.0> <0.255.255.255>
access-list <10> <permit/deny> host <3.3.3.3>
```

To apply standard ACL to interface for filtering:
```c 
interface <g0/0>
ip access-group <10> <in/out>
```

##### Extended ACL type Config
To define an extended ACL:
```c
// wild card mask
access-list 101 permit ip 10.0.0.0 0.255.255.255 any
// access-list 101 <permit/deny> <protocol> <IP range to IP> eq <port of protocols>
access-list 101 deny tcp any any eq 80  // 80 is "http"
access-list 101 permit ip host 3.3.3.3 any
```

To apply extended ACL to interface for filtering:
```c
interface g0/0
ip access-group 101 in
```


### ACL Verification
----
To list all ACL entries:
```c
show access-lists
```

To list a specific ACL's entries:
```c
show access-lists 10
```

To check if an ACL is applied to an interface:
```c
show ip interface g0/0
```


### Named ACLs
----
- Processed like a numbered ACL
- Identified by text name -Case Sensitive
- Easier to manage 
	- Sequence numbers allow for modificaiton of single ACL entries


### Named ACL Configurtation
---
To create a named ACL:
```c
ip access-list standard <MyACL>
10 permit host 1.1.1.1
20 deny 10.1.2.0 0.0.0.255
15 permit host 10.1.2.115   // adding this will actually squeeze this line in between the 10 and 20. This is why we label them 10,20,30,40 that way we can add new updates inbetween areas when needed.
```

To remove a single line:
```c
ip access-list standard MyACL
no 20 deny 10.1.2.0 0.0.0.255
```

To delete entire ACL:
```c
no ip access-list standard MyACL
```


### Determining what TYPE the ACL is. (standard/ extended)
---

```c
// For this example we are assuming these are seperate ACLs all we are anylising only the identification of the TYPEs

// Standard = 1-99, 1300-1999
Standard b/c the "70" -> ip access-list 70 deny 10.2.xx.8 0.0.0.255

// extended = 100-199, 2000-2699
Extened b/c the "120" -> ip access-list 120 permit tcp 10.2.xx.8 0.0.0.255
```


# Lecture11
----
### Subject: IPsec/Tunneling 
[back to top](#sections)


### In this Lecture
---
- Network Address Translation
	- Static 1-1
	- Dynamic NAT
	- The Two above Create a pool of IPs
- IPsec Tunnels
	- Create a logical P2P connection between two remote sites
	- Encrypted
- Set of Standards & protocols that support secure communication as packet are transported across network boundries


### NAT Referesher
---
- NAT allows a host that does not have a valid, registered, globally unique IP address to communicate with other hosts through the internet.
- NAT achieves its goal by using a valid registered IP address to represent the private address to the rest of the Internet.
	- Recorded in out NAT table
- The NAT function changes the private IP addresses to Publicly registered IP address inside each IP packet

![[Screenshot (54).png]]



### Static NAT
---
- Static NAT - A One-to-One mapping
- Requires one public address for each private address that needs to be translated

![[Screenshot (52).png]]


### Dynamic NAT
----
- Dynamic NAT
- Like static NAT, the NAT router creates a one-to-one mapping between an inside local and inside global address and changes the IP addresses in packets as they exit and enter the inside network
- However, the mapping of an inside local address to an inside global address happens dynamically.
- static = 1 to 1
- Dynamic = Set of pools inside global addresses and defines matching criteria to determine which inside IP address should be translated with NAT.

![[Screenshot (53).png]]


### Static NAT
----
CONFIGS:
```c 
interface g0/0
ip address 10.1.1.3 255.255.255.0
ip nat inside

interface serial0/0/0
ip address 200.1.1.251 255.255.255.0
ip nat outside

// ip nat inside source static <private IP> <public translation>
ip nat inside source static <IP> <public IP>  
ip nat inside source static <IP> <public IP>  
```


### Dynamic NAT
----
CONFIGS:
```c
interface g0/0
ip address 10.1.1.3 255.255.255.0
ip nat inside

interface serial0/0/0
ip address 200.1.1.251 255.255.255.0
ip nat outside

access-list <1> permit host <10.1.1.2>
access-list <1> permit 10.2.0.0 0.0.0.255

ip nat pool <fred> 200.1.1.1 200.1.1.2 netmask 255.255.255.252
ip nat inside source list <1> pool <fred> 
```


### IPsec Tunnels
----
- Tunnels create a logical point to point connection between 2 devices that are not directly connected.
- This is accomplished by utilizing existing valid routing paths
	- Overlay/Underlay
		- Underlay -> Physical network
		- Overlay -> logical network
		- Does not only encrypt & authenticate, but encapsulates packet into a new packet entirely.
- Segregates Traffic
- Encrypted 
- ##### Use Cases for tunnels
	- B2B tunnel connecting two physically distant LANs (business to business/ B2B)
	- Client VPN allows users remote access to a network.
	
	- Client VPN is used to allow remote users access to the network.
	- B2B VPN is used to connect two entities or sites within an entity.
		- You used a IPsec tunnel in your environment.
		- Tunneling traffic is generally used to allow traffic that is using private addresses to traverse the internet usually in site-to-site capacity. It can also be used during a Merger and Acquisition (M+A) where you might have two networks that use the same IP space, you can use a tunnel to traverse the overlapping network to prevent the need for NAT or complete IP addressing overhaul. You used a IPsec tunnel in your environment.


## IKE Phases 1 (IKE_SA_INIT)
----
- Initial tunnel built to manage the second phase
	- Authenticate IPSEC peers & set up out secure channel
- Establishes ISAKMP session
	- Internet Security Association and Key Management protocol 
		- Negotiation protocol that let our 2 hosts agree on how to build our IPSec AS's (Security Assocation)
- Only management traffic passes through the phase 1 tunnel
- Exchange of security Association (SAs) - MUST MATCH EXACTLY
	- Hashing Algorithm
		- HMAC (Hashed Mesage Authentication Codes)
		- Hashing ensures identity of sender & the integrity of our data
			- Remember testing the ISO images with the hash keys using grep.
	- Authentication Method
		- some different types:
			- RSA signature
			- Pre-shared key
	- Diffie Hellman Group
		- Magic happens here! MATH
		- Method of securely exchanging our cryptographic  keys over a public channels
		- sent publically but 
		- Group # defines strenght of encryption-key determines algorithm
	- Encryption Method 
Phases of phase 1: -> SA exchange -> Established -> Management Traffic flows


### IKE Phase 2
----
- Tunnel encapsulated inside of phase 1 tunnel
- Data flows through this tunnel from one end point to the other
- Seperate set of SAs -> MUST MATCH ON BOTH SIDES
	- Protocol - AH or ESP
	- Encapsulation Algorithm - DES, 3DES, AES
	- Authentication Algorithm - MD5, SHA
	- Perfect Forward Secrecy (Optional)

Phases of phase2: 
Management Traffic -> Tunnel Init -> Exchange SAs -> Tunnel up -> Traffic Flows


### IKE v1 vs v2
----
- V1 uses more bandwidth
- V2 supports EAP authentication
- V2 has NAT traversal built in
- V2 has a built-in keep alive mechanism to keep the tunnel up


### Encryption Types
----
- What is encryption?
	- The process of converting information or data into a code especially to prevent unauthorized access
- 3 Mehtods of encryption
	- DES - Least Secure
	- 3DES (Triple DES) - Medium Security
	- AES - most Secure
		- NSA is actively trying to break this encryption method


### Authentication Methods
---
- What is Authentication?
	- Verifyig the devices are who they say they are utilizing either a Preshared Key (PSK) or Certifcate - Verification of Authenticity by the Manufacturer
- 2 Authentication Mehtods
	- AH - Athentication Header 
		- unsecured
		- provides authentication only
			- HMAC-MD5
			- HMAC-SHA
	- ESP - Encapsulating Security Payload
		- Can be Secured
			- Uses same Hashing algorithms as HH but does not only authenticate outer IP header it looks at the IP datagram (data) portion.


### Transport and Tunnel Mode
---
- 2 Modes of operation
	- Transport Mode
		- Uses the original IP header
	- Tunnel Mode
		- Generates new IP header by encapsulating  existing packet
		- This mode is commonly used for site to site (B2B) VPNs die to needing to connect private address networks together 
			- DONT BREAK THE INTERNET
				- DO NOT SEND OUR PRIVATE IPs TO INTERNET NETWORK


### Hashing
---
- What is hashing
	- Hashing is the process of transforming any given key or a string of characters into another value
	- This is usually represented by a shorter, fixed-length value or key that represents and makes it easier to find or employ the original string
	- algorithm used to verify the integrity of packets
- Hashing methods
	- MD5
	- SHA -Utilizes Diffie Hellman (DH) Groups
		- Higher numbers are more secure, but more computationally expensive.
		- sha256 < sha512


### VTI (virtual Tunnel interface)
----
- Logical layer 3 interface
- Used as endpoint for tunneling protocols
- Can be statically or dynamically defined
	- Dynamic is typically used for hub and spoke topologies like in DMVPN


#### CONFIGURATIONS
----
Create ISAKMP Policy (Phase 1)
```c
crypto isakmp policy 1
encryption <AES>
authentication <pre-share>
group <16> // diffie hellman group
```

Set PSK for the Destination
```c
crypto isa kmp key <test> address <63.2.29.18>
```

Set the Phase 2 Parameters
```c
crypto ipsec transform-set <Aston_SIC> <esp-aes> <esp-sha512-hmac>
mode <tunnel/transport> 
```

Apply your policies
```c
crypto ipsec profile <Aston_SIC>
set transform-set <Aston_SIC>
```

Create the VTI
```c
interface tunnel<0>
ip address <192.168.29.1> <255.255.255.0>
tunnel source <g0/0>
tunnel destination <63.2.29.18>
tunnel mode <ipsec> <ipv4?
tunnel protection <ipsec> profile <Aston_SIC>
no shut
```

Create routing to your destination Subnet pointing to the tunnel
```c 
ip route 10.2.29.0 255.255.255.0 tunnel<0>
```


#### VERIFICATIONS
---
To test reachability and the path taken
```c 
ping/traceroute
```

Detailed view of local crypto endpoint and remote crypto endpoint, used to establish if tunnel has been created and is functioning
```c
show crypto ipsec sa
```

show the souce, destination and status of the tunnel
```c
show crypto isakmp sa
```

### Whats the ipsec: TO CREATE A logical peer to peer connection between sites while securing traffic


### Side Notes
-----
[back to top](#sections)

#### You can add a `description` to every interfaces
- [More about descriptions here](#obsidian://open?vault=network%20fundementals&file=Descriptions%20for%20interfaces)