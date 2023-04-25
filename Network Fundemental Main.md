## Sections: 
[Lecture1](#lecture1) -> Intro Lecture OSI
[Lecture2](#lecture2) -> Basic Switching
[Lecture3](#lecture3) -> Basic Layer3 
[Lecture4](#lecture4) -> General Routing
[Lecture5](#lecture5) -> Spanning Tree Protocol 
[Lecture6](#lecture6) -> EIGRP
[Lecture7](#lecture7) -> OSPF
[Lecture8](#lecture8) -> BGP/PAT
[Lecture9](#lecture9) ->
[Lecture10](#lecture10) ->


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

### Reserved IP Addresses 
-   Network IP 
-   Broadcast IP

## Lecture2
----
[back to top](#sections)
### Subject: Basic switching
Overview: Broadcast, Vlans, SVI's, Switchport modes, packets & frames, Mac Address tables.

### CDP, LLDP = Layer 2 network discovery protocols

### Duplexes (talking about wiring configs):
- Full Duplex (***Bidirectional***):
	- Simultaniously transmits & receive data
	- seperate wires for transmiting and receiving
	- TX -> Transmit
	- RX -> Receive

- Half Duplex:
	- Can only transmit or receive data at one time
	- Share wire for RX and TX. 
 
![[duplexes.png]]

### Domains:
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

### VLAN (Virtual Local Area Network):
- PURPOSE -> Limit broadcast domains
- Logically created (Created with software)
- Layer2 segmentation of a switch -> Limit broadcast domain.
- Locally ***significant to*** a ***switch***
- Each vlan is its own broadcast domain
- Minor security function
- DO NOT USE VLAN 1 -> Because it is CISCO DEFAULT & is bad practice
- Range 1 - 4096 unique VLANs

##### VLAN Configuration:
- For each VLAN you need to create (1 - 4096)
```c
vlan <VLAN_ID>
```
- name 
```c
name <NAME>
```

##### VLAN Verification: 
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

### Switch Port Modes 
- Only pertains to the Switch
- ONLY 2 OPTIONS:
	- Access
	- Trunk

##### Access:
- Single Vlan -> Part of only one vlan
- Typically connects to hosts/ end devices
	- ACCESS MODE WILL NOT CARRY VLAN TAG
- Associates data to VLAN

##### Trunk:
- Multiple VLANs -> Carry one or more VLANs on the same physical link
- Uses 802.1q encapsulation
	- encapsulates packets using 802.1q protocol with a TAG to identify the VLAN on layer2 
- typically connects to other switches
- connects VLANs between switches
- TYPE OF INTERFACE CONFIGURED ON A SWITCH OPPOSITE A ROUTER'S SUB-INTERFACE. (REMEMBER SUB-INTERFACES ONLY EXIST ON A ROUTER-ON-A-STICK ARCHITECTURE'S)

### Configurations:

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

##### Access Port Verification commands:

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

##### Trunk port config commands: 

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

##### Trunk Port Verification commands:

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

#### Switch Virtual Interface ( SVI ):
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

### Packets & Frames:

#### Layer 3 Packet (Smaller then Layer2 Frame) (IP):
- source IP address
- Destination Address
- TTL : L3 = loop prevention mechanism
- Payload

![[20230412_131156.jpg]]

#### Layer 2 Frame (bigger then layer 3 packet)(MAC):
- Destination MAC Address
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
- ***Ties physical port to MAC address.***
- Unique Physical identifier
- Updates when frames com into port
- ***Makes intellegent decisions about where traffic is going***

![[20230412_132618.jpg]]

##### Commands :

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
- ***Unicast***
	- 1 to 1
	- private direct message
- ***Broadcast***
	- 1 to all
	- global message to all connected devices on vlan

not as important
- Flood
	- 1 to all*
	- special use case
- multicast
	- 1 to many*

### Device Access:
-how we connect to communicate with devices

- Out-of-band
	- Physical connection
	- Console cable
- In-band (Loop for router in layer3, SVI for switch in layer2)
	- Virtual connection
	- SSH/Telnet

### ICMP commands (internet control message protocol)

- Tests L3 connectivity between 2 hosts
	- source host sends a ***"ping"***
	- Destination replies with a "ping"

##### Syntax:
switch
```c
Switch# ping <IP_ADDRESS>
``` 

traceroute DONT FORGET ABOUT THIS ONE, VERY USEFUL
```c 
Traceroute# traceroute <ip_address> (cisco ios)
```

windows
```c
windows# tracert
```

linux
```c
LinuxUser$ traceroute
```


## Lecture3
----
[back to top](#sections)

### SUBJECT: BASIC LAYER 3

[TEST PREP](obsidian://open?vault=network%20fundementals&file=Leture%203%20quiz%20pre-review)

### TABLES:
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

### ARP (adress resolution table):
- Protocol that populates the ARP table
- How a device knows which MAC address to use
	- MAC address are 100% necessary for sending traffic
- ARP cache contains a record of each MAC mapped to each IP

Photo note: 
- if MAC address is a F's then it is a broadcast address. 
	- I.E MAC -> FFFF.FFFF.FFFF.FFFF
	- this is like the broadcast IP but on layer2

![[20230413_101818.jpg]]

![[20230413_101855.jpg]]
### Default Gateway:
- Hosts need a default gateway to send traffic outside of thier own subnet
- Gateway must be an IP on the local subnet
- Packets destined for a different network are sent to the gateway
- First Layer3 device on the VLAN network is the gateway. 
- Ip address we utilize to send local traffic off to another network

### Host Configuration:
- IP address- host ID
- Subnet mask - defines network
- Default gateway - how to leave the network

syntax: 
```c
ip <10.0.0.4> <255.255.255.248> <10.0.0.1>

// ip <IP> <subnet> <default gateway>
```

### Routing Between Subnets
- 3 Methods:
	1. Router connected to 2 switches
	2. Router-on-a-stick
		1. SUB-INTERFACES ONLY EXIST IN THIS METHOD
			1. SUB-INTERFACES ARE USED TO LEVERAGE CONFIGURATION OF MULTIPLE INTERFACES ON A SINGLE PORT (INTERFACE).
	3. inter-VLAN routing using a Layer3 switch/Multilayer switch

# ! 
( Trunk is a configuration inside a switch where router on a stick is a topology and not the same. )

#### 1. Router + 2 switches:
![[20230413_103427.jpg]]

#### 2. Router-on-a-stick:
![[20230413_103444.jpg]]

- ##### Sub interfaces are only used on router-on-a-stick. The other subnet routing methods do not use sub interfaces.

- **Sub interface configuration:**
	- Configure the physical interface
```c
interface gigabitEthernet 0/0
```
```c
no ip address
```
```c
no shutdown
```

- Configure virtual interfaces
```c
interface gigabitEthernet <0/0.10>

// <interface/number.VLAN_ID>
```
```c
encapsulation dot1q <10>
// <VLAN_ID>
```
```c
ip address <10.10.0.1> <255.255.255.0>
// ip is referencing the ip associated with the above VLAN
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


#### 3. inter-VLAN routing using a Layer3 switch/Multilayer switch
![[IMG-20230414-WA0008.jpg]]
##### Layer 3 Switches/Multilayer
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

[Picture of inter-VLAN routing]()

##### L3 Switch  Enable /Configuration: 
- If the switch is able to do layer3 routing you will need to enable the routing feature:

Enable routing on switch:
```c
ip routing
```

# !
**NEED TO CONFIGURE VLANS FIRST
```c
// This needs to be declared before configuring SVI regardless of L2 or L3
vlan <1290>
name management
```
# !

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

##### L3 Switch Verification:

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
- Loopbacks are for remote management
- Logical interface primarily used for managment purposes
- Always in UP/UP state
- Best practice is to use a /32 subnet
	- you want the loopback on its own network so you are using the /32 for the subnet because you eliminate the possibility of anything else on the network. 
		- Remember /32 only has one host on the network.

##### Loop back interface configurations:

Create new loopback interface:
```c
interface loopback <0>
```
```c
ip address <10.1.1.1> <255.255.255.255>
// CIDR /32 subnet
```

##### Loopback interface verification:

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

## Subject: General Routing

##### Overview: 
- Routing Table
- Route lookup
- Longest Match Rule
- Static routes
- L3 switch routed interfaces
- Cisco Discovery Protocol/LLDP
- internet Control Message Protocol ICMP
- Route summerization

ARP -> all layer3 devices will have a ARP table

## What is routing?
- Moving packets - containing data - through a network
	- Process of moving packets through out network
- #### Source & Destination
	- #### routing relies on ***source/destination*** IP's
- Routers route many, many packets per second

## Routing table:
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
- Administrative Distance (TRUST)
	- Arbitrary number that determines the preference of the route
	- ***LOWER NUMBER IS PREFERED (MORE TRUSTED)
		- This rule is opposite then the Longest match rule
		
[Image of Routing table]()

(S means the IP was learned "statically")
(C means the IP was learned "connected") !!!! Need clarification on connected and how it is obtained?
(S* "static" default route, last resort. You do not want to rely on this route)

## Administrative Distance 
- go [here](#why%20admin%20distance?) for what admin distance is doing

[image of adim distance chart
![[20230414_105217.jpg]]
[route lookup process image]

(router will drop the packet if it cannot find the IP in the table)

## Longest Match Rule
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

PROS AND CONS IS GOING TO BE ON THE TEST
- Manually add routes to routing table
- A few Cons:
	- Lack of scalability
	- Lack of redundancy
	- Higher chance of human error
	- High administrative overhead
- A few Pros:
	- Lower CPU usage
	- Mildly more secure
		- less protocols that can be exploited.

Hacking tip:
- One can inject IPs into a network that has dynamic routing to redirect there traffic. 

## Static routing Requirements
- The next-hop IP address must be accessible, i.e. in a connected subnet
- Valid Destination Network address
- Valid Subnet size

 - ##### All 3 requirements must be met for a route to be entered into the routing table.


### default route
- This is a route that matches all routes
	- notation: 0.0.0.0/0
	
## Static routing Parameters:
- 3 Mandatory parameters:
	- Destination network
	- Destination subnet mask
	- Valid Next-hop IP address

		- Destination network address 

- Optional parameters:
	- Name
	- Admin distance
		-  Static route AD = 1

#### Static routing configurations
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

##### why admin distance?
- when talking about routing protocols there are default administrative distances. Static routing protocol is a defualt of 1 where OSPF default is 110. So we use AD (administrative distance) to be able the configure a different routing protocol. The trusted protocol with AD is the lower value.
- So to change the routing protocol from "static routing" to "OSPF" we increase static routing to any value above 110 (OSPF). In the examples of commands above we the last to last values are changing there administrative distance to select the desired routing protocol

Defaults
- static routing = 1
- OSPF = 110

#### Static Routing Verification:

To show configured static routes:
```c
show running-config | include ip route
```

To show static routes installed in routing table:
```c
show ip route static
```

## L3 Switch Routed interfaces
- WHAT DO YOU NEED FOR L3 switches to work
	- #### ***IP routing must be enabled***
	- ##### multiple SVI's
- Behaves just like a port on a router
	- NO sub-interfaces
- Is not own IP address
- Doesn't participate in STP
	- STP = Spanning Tree Protocol -> Look into this!

- have to explicitly say that the L3 interface is not a switchport via:
```c
no switchport
```

#### L3 Switch Routed Interface Configurations

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

## Cisco Discovery Protocol
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

To show a list of all directly connected neighbors:
```c
Show cdp neighbor
```

To show detailed information about neighbors:
```c
show cdp neighbor detail
```

## Link Layer Discovery Protocol (LLDP)
- LLDP
	- Open source L2 discovery protocol
	- Must be enabled/running to discover devices

#### LLDP Verification commands:

```c
show lldp neighbors
```

```c 
show lldp neighbors detail
```

## Internet Control Message Protocol (ICMP)
- Ping

verifcation  needs to go here

trace route needs to go here

## Route Summerization:
- Route summerization aka supernetting is the process by which you take 2 or more smaller subnets and combine them into one larger subnet
- Done to reduce admin overhead
	- Can black hole unintended traffic
		- Black holing does not give a error
		- 
- Having a smaller router table will help speed up our routing process I& use less system resources

## KEY CONCEPT FOR SUPERNETTING
- You can combine two subnets if they are continuous (in sequential order) this process is summerizing the route. AKA route summerization.
	- if you have two seperate subnets that are continuos like so
		- 192.168.20.0/25 - Host range 0 - 127 (128 hosts) (128 - 2 =126 usable)
		- 192.168.20.128/25 -> Host range 128 - 255 (128 hosts) (128 - 2 =126 usable)
	- The above are two seperate subnets can be combined into one subnet that can contain the range for both subnets. the following is the route summerization we can use as our route summerization
		- 192.168.20.0/24 -> Host range 0 - 255 (256 hosts) (256 - 2 = 254 usable)


# Lecture5
----

Subject: Spanning tree Lecture

### QUIZ NOTE:
What is the command to show all interface status's on any cisco device?

```c
show ip interface brief
```

when a request it willl record its MAC address on its table and the port number that associates with the MAC address

## Packet and Frame review:
- Frames do NOT have TTL (no loop prevention mechanism like with packets ) < Can cause broadcast storms

[picutre o packet and frame]
[picture of storm]

## Spanning tree protocol (STP -> Layer2)
- Layer 2 protocol
- purpose: to prevent Layer2 Loops (broadcast storms)
- Method: logically block redundant links
-  STP -> build loop-free logical topologies for our network

#### How does stp work?
- First, a root bridge must be elected -> Root bridge = King/Queen of topology
	- **Root bridge** = Swtich with the lowest BID
		- root bridge means it makes all decisions on what ports are open and which ones are blocked. Remember this is all to prevent broadcast storms. That is all.
	- **BID** = Bridge ID; priority + MAC address
		- bid = 36768 + mac
	- **Priority** = arbitrary number to influence root bridge election; 
		- ### Defualt value = 32768
			- If default prioity is selected a older switch (Older switch = one manufactured with a lower mac address number then the newer ones) will take priority before the new switch which would be undesirable because we want the newer more powerful switch to be our root. 
		- Priority value range:
			- 0 -> 61440
			- increments in steps of 4096
		- This number we can influence to make the root what we want.
			- setting the priority to "0" will garuentee that switch becomes our root bridge.

#### STP Root Election:
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

[picture of root bridge example]
Root bridge is the switch on the lower right. -> Lowest BID number.

#### STP Port Roles (for determining what ports are blocked or deticated)
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
[picture of STP port roles]

[link cost picture STP port speed costs]
- ####  STP Port Speed Costs
	- Faster link is the LOWER link cost.
	- standard numeric values 
|SPEED|STP PORT COST|
|---|---|
|10 mbps|100|
|100 mbps|19|
|1 gbps|4|
|10 gbps|2|
|10+gbps|1|



[pictured example of how to determine root ports, designated port, blocked port]

#### STP Port States: (ALL PORTS GO THROUGH THESE PHASES)
- Blocked (20s)
	- Blocked port, but able to recieve BPDUs -> still up for redundnacy 
	- Stable state listening for BPDUs/ does not forward Data
- Listening (15s)
	- Blocked , but communicating via BPDUs
	- Tansitional state, listening and sending BPDUs
- Learning (15s)
	- Blocked, but allowing traffic to populate MAC table
	- Transitional state, listening/ sending BPDUs, Learns MAC address but does not forward data
- Forwarding 
	- Normal forwarding port
	- stable state listening/sending BPDUs + Forwards data.
	
- WE CAN SKIP THE 50 SECONDS THIS PROCESS TAKES BY PROGRAMMING OUR PORTS TO SKIP BLOCKED, LEARNING, AND LISTEN PHASE. - Spanning tree Portfast

### Newer Versionds of STP
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
- portfast is an extention of the STP protocol and not the STP its self.
- "Access" ports only 
- Spanning Tree Portfast
	- Forces port to skip all states and go directly into forwarding
To configure: Spanning-Tree portfast:
```c
// global port portfast selection on all ports
spanning-tree portfast default

// individual port portfast selection
int g0/0
spanning-tree portfast

// remove spanning-tree portfast
int g0/0
no spanning-tree portfast
```

- BPDU Guard
	- Only used sometimes
	- Used in conjunction with portfast
	- "if you happen to receive a BPDU from this port, go directly into err-disable mode."
		- Err-disable -> logically blocks port
		- To recover from err-disable, "bounce" the port (shut/no shut)

```c
// global configuration of BPDU gaurd
spanning-tree bpdu gaurd default

// individual config of BPDU gaurd
int g0/0 
spanning-tree bpdu gaurd
```

# Lecture6
----
[back to top](#sections)

### Subject:  EIGRP

Glossary: 
- Redistribute -> share IP networks with neighbors

## Dynamic Routing
 - Allows network topologies to dynamically adjust to changing network conditions.
- pros:
	- Full redundancy
	- Scalability
		- if we add a new route it will automatically share that route with its neighbors.
	- Easy to manage
- Cons:
	- Less control
	- More complex

#### Dynamic Protocols
- EIGRP -> Enahnced Interior Gateway Routing Protocol
	- cisco proprietary
- OSPF -> Open Shortest Path First
	- open source
- BGP -> Border Gateway Protocol

### Dynamic routing types:

## TEST Question:
EIGRP = Distance Vector
OSPF = Link-State

1. Distance Vector -> EIGRP USES THIS
	- Distance (length) + Vector (direction)
	- ##### Also known as "routing by rumor"
	- A device operating with distance vector protocol is ***unaware of the network topology.***
		- Meaning neighbors tell the device these exist but the current router does actually know it just trust this to be true.
		- These devices only know about its directly connected networks & the remote networks it can reach via its neighbors
2. Link-State -> OSPF uses this.
	- Complete knowledge of network
		- Meaning that a device operating with a link-state protocol will build a full topology of the network. 

## EIGRP: 
- Enhanced interior Gateway Routing Protocol
- ### Distance vector protocol
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


## Network Statements:
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

##### NETWORK STATEMENTS TELL WHICH INTERFACES CAN SEND AND RECEIVE TRAFFIC

#### EIGPR NEIGHBORS: 
- Connected L3 devices that share their routes with each other
- #### Relationship is formed through matching network statement
- Parameters that MUST match:
	- Autonomous system number
	- Subnet mask
	- THESE DEVICES ARE DIRECTLY CONNECTED SO THE "NETWORK STATEMENTS" ARE IDENTICAL BETWEEN EACH OTHER. WE NEED TO JUST FIGURE IT OUT ONCE THEN ADD THAT ONE NETWORK ADDRESS TO BOTH ROUTERS TO MAKE SURE THEY MATCH.
- if parameters do NOT match:
	- No neighbor relationship is formed

### EIGRP CONFIG

Create the EIGRP process:
```c
router eigrp 50  // 50 = AS, ASN number
```

Add interfaces to the EIGRP AS:
```c
network 10.2.2.2 0.0.0.3    // some port
network 10.1.1.1 0.0.0.255  // some other port not on same network above  
```

### PASSIVE INTERFACES:
- Interfaces you do NOT want to form neighbors on
- If you write a network statement containing the subnet of the passive interface:
	- Interface subnet will still be added to the EIGRP topology table 
	- Interface will NOT send "Hellos"
- You explicitly program the interfaces on layer 3 that connect layer 2 devices  because we do not wish to send hello broadcasts to the layer2 devices.

#### Passive interface configurations:

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
- Default route must already be in the routing table

To redistribute: -> share IPs with neighbors
```c
// ip route <default network address> <subnet> <next hop>
ip route 0.0.0.0 0.0.0.0 10.4.6.1

router eigrp 50

redistribute static       // add configured static routes to topology -> WILL TAKE PRIORITY IF YOU DO NOT CHANGE THE AD (ADIMINISTRATIVE DISTANCE)

redistribute connected    // adds all port configured IPs to topology.
```


### REDISTRIBUTIONS:
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

#### EIGRP REDISTRIBUTION CONFIGURATIONS:
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

#### EIGRP REDISTRIBUTION VERIFICATION:

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
[back to top](#sections)

QUIZ:
- What command could you use to add (convert) all svi interfaces on a particular switch to OSPF
```c
redistributed connected subnets
```

### NOTE: 
- Satic routes have a lower default Admin distance (AD). If you do not pay attention the router will send data down lines with a higher cost instead of the shortest path. 
	- Static routes = 1
	- ospf =110
Need to configure the AD on the statics to be higher then the protocol desired.

### Subject: OSPF

[CHEAT SHEET](https://packetlife.net/media/library/10/OSPF.pdf)

### Open Shortest Path:
- #### Link-state protocol 
	- builds entire image of topology. Does not rely on a rumor from a neighbor.
- OSPF process ID
	- not ASN like EIGRP 
	- locally significant to system / where is ASN are globally unique.
- Admin Distance (AD) = 110 (higher than EIGRP)
- Each router calculates best path for data based on the complete knowledge of the network.
	- uses -> dijkstra's algorithm

### OSPF AREAS
- Segment into routing hierarchy
- Limit impact of network changes
- Area 0 (aka "backbone area")
	- Must exist
	- #### All other areas must attach to Area 0

### OSPF Router Types
[picture of OSP ROUTER Types]

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

### OSPF CONFIGURATION:

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

### OSPF Database:
- Collection of all the LSAs for an area (Link state advertisments)
	- Link state advertisements
	- how routers tell the other router about the routes it knows of. 
- Shared via LSUs (Link State Update)
	- Like the car that carries the LSAs to there location. LSA is like the person in the car (LSU)
- For each area:
	- Unique Database
	- Each router has complete copy
	- ABRs have a database for each area

### Passive Interfaces:
- Interfaces you do NOT want to form neighbors on
- If you write a network statement containing the subnet of the passive interface:
	- Interface subnet will still be added to the OSPF database
	- Interface will NOT send "Hellos"

#### QUIZ -> If You were asked what are some RESTRICTIONS you can put on an interface for OSPF -> 
```c
passive-interface <port #>
```

### Passive-interface Configurations:

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

##### OSPF Default information:
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

##### Redistrubution configuration:
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

### LINK STATE ADVERTISEMENTS

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

### OSPF Path Selection:
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

### OSPF VERIFICATIONS:

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
[back to top](#sections)

## Subject: BGP/PAT

#### Prefixes = IP address with associated subnet mask

### NOTE: Any interface you wish to access from any outside network must be explicitly labeled as `ip nat inside` in order to trigger the NAT an allow communication.

#### BGP
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
#### PAT
- Port Address Translation (PAT)
	- Translating IP addesses from Private to Public while minimizing Public IP usage.
	- attaches port numbers to few IPs to make them unique. In turn minimizing the number of IPs needed. also allows translation to attach a private IP to the IP & port number for communication.

### BGP
- exterior Gateway Protocol
- primary function of BGP is to exchange layer 3 reachability info with other BGP routers
- Routing protocol of the internet
- TCP Port "***179***"
	- This protocol is where sessions get connected. But is a layer 3 protocol
- "Path Vector" protocol
	- AS hop
	- BGP = path vector unlike eigrp = distance vector or ospf = link-state, 
	- calculates best path by the least amount of HOPS from one autonomous system to another (meaning network to network not router to router). It does not take into acoount the cost of the hops.
[picture of GBP hops from AS to AS]

### AUTONOMOUS SYSTEM NUMBERS
- Each group of routers running BGP will utilize an autonomous system number
- Valid AS number range 0 - 65,535
	- 0: Reserved 
	- 1 - 64,495: Public AS numbers
	- 64,496 - 64,511: Reserved to use in documentation
	- 64,512 - 65,534: Private AS numbers
	- 65,535: Reserved
- There are 6 different areas in the world that will run a curtain range fo these AS numbers. You can find out the area of the world this AS is being used just by referencing this number.

### BGP Table
- List of "***PREFIXES***" known by BGP
- "***Prefixes***" shared to peers
	- Technically, BGP does not announce or share routes. 
		- It actually announces the "***PREFIXES***" for route sharing.
	- "***Prefixes***" are added to the local BGP table via network commands
- Add new routes from routing table
- "***FILTERING***" routes in BGP we utilize BOTH "***prefix-lists***" & "***route-maps***"

### NETWORK STATEMENTS
- Must match an EXACT prefix
- add/inject routes to the BGP table from Routing table
- Doesn't interfere with neighbors like IGPs
- NO wildcard logic (subnet mask syntax)
- network commands work differently in BGP compared to ospf and eigrp
	- It specifies what network to announce.
##### QUIZ 
- 2 ways to inject the routes  into BGP
	1. Network statements
	2. Redistribution

### ADVERTISE SUMMARY NETWORKS
- ##### Exact prefixes must exit in routing table
	- use a null route
		- null route are "fake" routes that we use to add summerized networks to our routing table for BGP
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
- Advertise routes learned from different sources on same router
[picture of mutual redistribution]
- When a single router operating with 2 or more dynamic routing protocol redistributes routes between the separate dynamic protocols
```c
router ospf 10
redistribute bgp 50
```

### eBPG vs. iBGP
- External BGP (eBGP)
	- Outside the same AS -> different network to different network
	- BGP peering between routers in different AS
	- Admin Distance = 20
- Internat BGP (iBGP)
	- inside same AS -> working inside the same network
	- BGP peering between routers in same AS
	- Admin Distance = 200
		- Not trusted nearly as much as eBGP
		- will opt for eigrp or ospf before opting in on iBGP
	- Full mesh requirments
		- each router has a wire going to each router.
		- full mesh -> also known as split horizon

### CONFIGURATIONS:

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

#### VERIFICATIONS:

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
- NAT allows a host that does not have a valid, registered, globally unique IP address to communicate with other hosts through the internet.
- NAT achieves its goal by using a valid registered IP address to represent the private address to the rest of the internet.

- The NAT function changes the private IP addresses to publicly registered IP addressess inside each IP packet.
[picture of NAT]

### NOTE: Any interface you wish to access from any outside network must be explicitly labeled as `ip nat inside` in order to trigger the NAT an allow communication.

### (Overlaod) PAT
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

[picture of pat cont.]

##### (OVERLOAD) PAT cont. CONFIGURATIONS
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
### NOTE: Any interface you wish to access from any outside network must be explicitly labeled as `ip nat inside` in order to trigger the NAT an allow communication.

Reverse NAT setup

```c
// OUTSIDE is case sensitive and is just a label for the internet.
ip nat pool OUTSIDE 10.2.12.249 10.2.12.254 netmask 255.255.255.248

ip nat outside source list 1 pool OUTSIDE add-route  
```

#### NAT VERIFICATIONS
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
[back to top](#sections)


# Lecture10
----
[back to top](#sections)


# Lecture11
----
[back to top](#sections)