
## Sections:
----
Basic:            [FindThings](#findthings) | [Modes](#modes) | [TroubleShoot](#troubleshoot) | [Section](#section) | [InterfaceRanges](#interfaceranges)

Devices:        [InitialConfig](#initialConfig) | [SwitchConfig](#switchconfig) | [L3 Switch](#l3%20switch) | [Router](#router) | [Port Channel](#port%20channel)

Interfaces:    [Access Interface](#access%20interface) |  [Trunk](#trunk) 

Protocols:     [EIGRP](#eigrp) | [OSPF](#ospf) | [BGP](#bgp) | [ACL](#Access-List%20(ACL)) | [Prefix Lists](#prefix%20lists) | [Route Maps](#route%20maps) | [NAT/PAT](#nat/pat) | [HSRP](#hsrp) | [DHCP](#dhcp) |  

ALL:              [ALL VERIFICATIONS](#all%20verifications)

Operating system Commands: [Network Command Comparison](#network%20command%20comparison) | [Linux](#linux) | [Windows](#windows)

----

## FindThings
----
[back to top](#sections)
```c
// find all commands 
show ?
// The examples below use interfaces as an arbitrary example anything can be used.
// find a part of a command you might be missing
// this will show anything that can go infront of the interfaces argument
show ? interfaces

// This will show anything that is connected to the end of the interfaces
// note the non space after interfaces. it will produce results like 
// show "interface-list"
show ip interface?

// This will show anything I can put after the argument
// note the space after the interface compared to the above command.
show ip interface ?
```


## Modes
----
[back to top](#sections)
```c
wgsIL029a02a01>                 // User Exec mode -> Very limited
wgsIL029a02a01#                 // Priveldge Exec mode -> can run show commands + save
wgsIL029a02a01(config)#         // Global Exec mode -> can write configs
wgsIL029a02a01(config-if)#      // explicit interface config
rtrIL029a02a01(config-router)#  // protocol config modes global
```


## TroubleShoot
----
[back to top](#sections)
```c
// To show running configurations of the interface:
show running-config interface <fastEthernet 0/2>
show run                  // shows all configs

trace
traceroute <ip>
traceroute <ip> source loopback 0
traceroute <ip> source <use outgoing interface as source ip>

show ip protocols         // VERY USEFULL FOR NETWORK DISCOVERY

ping <ip>
```

## Section
-----
```c

// show run | section <the section of run you wish to view>

show run | section ospf
show run | section eigrp
show run | section bgp
show run | section nat

```

### InterfaceRanges
-----

```c
// If you want to configure a range of interfaces you must use the RANGE keyword
interface range g0/0-4
```

## InitialConfig
----
[back to top](#sections)
```c
enable
conf t
hostname <name you want>
enable secret <password you want>
username <username you want secret <password>
line con 0
logging synchronous
login local
line vty 0 4
logging synchronous
login local
```


## SwitchConfig
-----
##### ^
Sections: [VLAN](#vlan) | [SVI](#svi) | [Trunk](#trunk) | [Access Interface](#access%20interface) | [Port Channel](#port%20channel)

##### VLAN
----
[back to top](#sections)
[back to switch](#^)
```c
vlan <number 0-4096>
name <arbitrary name>
```

##### Options
----
```c
no ip routing
```

##### SVI
----
##### VLAN -> NEEDS TO BE DECLARED FIRST
[back to top](#sections)
[back to switch](#^)
```c
// SVI Creation
// ------------------------
interface vlan <numer of vlan>
description <name>
ip address 10.2.29.214 255.255.255.248  // ip you will use to remote into switch
shut
no shut
```

##### Trunk
----
[back to top](#sections)
[back to switch](#^)

CONFIG
```c
// CONFIG

// when doing ranges you can use the commas ( 10,20 ) with no space inbetween or you can // you the hyphen ( - ) to seperate the ranges.
interface <port 0/2>
switchport trunk encapsulation dot1q
switchport mode trunk                           // must come before trunk allowed line
switchport trunk allowed vlan <10,20,30-40,50>
shut 
no shut
```

VERIFICATION
```c
// VERIFICATION

// To list all trunk interfaces and the allowed vlans:
show interface trunk

// To show trunk status and encapsulation type:
show interface <f0/2> switchport
```

### Access Interface
----
[back to top](#sections)
[back to switch](#^)

```c
// CONFIGS

int g0/2
description toJames
switchport access vlan 1291
switchport mode access
// you will want to make the ports not connecting to another switch to portfast while 
// using spanning-tree -> this makes the port go straight to fowarding. and skip the 
// other port states (blocked, listening, learning)
spanning-tree portfast
spanning-tree bpduguard enable
shut
no shut
```


### Port Channel
----
[back to top](#sections)
[back to switch](#^)

CONFIGS:
```c
// setting up the interface 
interface port-channel <g0/1>
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

//****************************************************
// Example
// PORT CHANNEL
// Create Trunk first. NOTE THE RANGE USE
interface range g0/1-2
description toWLS_01_WLS02
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 90,3290
channel-group 1 mode active  // This attaches the interfaces to the below channel conf
shut
no shut

// PORT 1 CHANNEL Creation
interface port-channel1     // This is new syntax
description toWLS_01_WLS02
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 90,3290
channel-group 1 mode active 
shut
no shut
```

VERIFICATIONS:
To verify the port-channel status: (JUST CHEKS FOR UP UP STATE)
```c
show ip interface brief
```

To list the stats of each individual member: (THIS WILL BE MOST USED)
```c
show etherchannel summary
```


## L3 Switch
-----
[back to top](#sections)

MUST ADD THIS TO ENABLE ROUTING
```c
// MUST enable to allow routing
ip routing
```

- SVI on Layer 3 are the default gateways for the VLANs attached to them.
```c
// initial config here

ip routing   // enables the L3 routing ability

// VLAN configs must 
vlan 1290
name Office1

vlan 2290
name Office2

vlan 3290
name Server

// ROUTED link Interface
// -------------------------
// TO_ROUTER

interface g0/3
no switchport     // MUST PUT THIS
ip address 10.2.29.201 255.255.255.252
shut
no shut

// LoopBack configurations
// -------------------------
interface loop0
ip address 10.2.29.194 255.255.255.255
shut
no shut

// SVI -> defualt gateways
// -----------------------
interface vlan 1290
description Users: office1
// address on LLD next to L3 Switch VLAN 1XX0: 10.2.XX.209/29
ip address 10.2.29.209 255.255.255.248   
shut
no shut
  
interface vlan 2290
description Users: office2
// address on LLD next to L3 Switch VLAN 2XX0: 10.2.XX.217/29
ip address 10.2.29.217 255.255.255.248
shut
no shut

interface vlan 3290
description Users: Server
// address on LLD next to L3 Switch VLAN 3XX0: 10.2.XX.225/27
ip address 10.2.29.225 255.255.255.224
shut
no shut

// Static Routes go here

// ospf config goes here
```


## Router 
---- 
[back to top](#sections)

CONFIGS:
```c
// Router: rtrIL029a02a01
// -------------------------------------------
// Init config
// -------------------------------------------

enable
conf t
hostname rtrIL029a02a01
enable secret cisco
username cisco secret cisco
line con 0
logging synchronous
login local
line vty 0 4
logging synchronous
login local
  
// LoopBack configurations
//-------------------------

int loop0
ip address 10.2.29.192 255.255.255.255
// WARNING If the Loopback is not set to inside nat it will not activate the translation
ip nat inside  
shut
no shut

// Interface CONFIGS:
//--------------------------
// toCOMCAST
int g0/1
ip address 63.2.29.18 255.255.255.0
ip nat outside  // Translates private IPs to public IPs
shut
no shut

// toNY
int g0/2
ip address 10.2.29.74 255.255.255.252
ip nat inside
shut
no shut

// toILL3SWITCH
int g0/3
ip address 10.2.29.202 255.255.255.252
ip nat inside
shut
no shut

// toCA
int g0/4
ip address 10.2.29.70 255.255.255.252
ip nat inside
shut
no shut

// toILL3SW02
int g0/5
ip address 10.2.29.206 255.255.255.252
ip nat inside
shut
no shut
  
  

// OSPF
// Defualt information orginate sends default gateway to all interfaces
// PID -> used to distinguish between multiple OSPF processes running on the same router
//--------------------------------------------------------------------------------------
router ospf <process ID>                // locally significant PID
network 10.2.29.200 0.0.0.3 area 0  
network 10.2.29.68 0.0.0.3 area 1  
network 10.2.29.72 0.0.0.3 area 2
// allows info from bgp to be distributed to other routers participating in ospf
redistribute bgp 129                   
default-information orignated          // note above

  

// BGP
//-----
// ADDED redistribute bgp 129 to the router ospf 20.
router bgp 129                        // 129 is the AS (autonomous system number)
neighbor 63.2.29.17 remote-as 7922    
network 63.2.29.0 mask 255.255.255.0  
neighbor 63.2.29.17 soft-reconfiguration inbound // Starts filter tracking
// The above code `soft-reconfigurtation` is necessary to allow logging of data before & after the bgp filter to allow diagnostics
// accross the filter. If it is not filtering properly you can see how the data enter compared to how it exits.


// GLOBAL NAT POOL ASSIGNMENT
ip nat pool OUTSIDE 10.4.29.0 10.4.29.254 netmask 255.255.255.0
ip nat inside source list 1 interface GigabitEthernet0/1 overload
ip nat outside source list 2 pool OUTSIDE

// ACL -> done inside global config. (ACCESS CONTROL LIST)
access-list 1 permit 10.2.29.0 0.0.0.255  
access-list 2 permit 180.3.29.0 0.0.0.255

// EIGRP Configs
router eigrp <29>            // The 29 is the AS number assigned to EIGRP/ not arbitrary  
network 10.3.29.72 0.0.0.3   // network statemente, uses wildcar subnet notation   
passive-interface g0/1
redistribute static
redistribute connected
redistribute bgp 229
default-metric 10000 10 255 1 1500  // Do not change this number.
// static ip routes
ip route <network address> <subnet mask> <next hop IP>
```



### EIGRP 
-----
[back to top](#sections)

CONFIGS
```c
// CONFIGS

router eigrp 29
network 10.3.29.72 0.0.0.3          // network statements must match
redistribute static                 // sends all static routes to its neighbors
redistribute connected              // sends all interface networks to neighbors
default-metric 10000 10 255 1 1500  // Never change this value

// Redistribute OSPF 10 into EIGRP:
redistribute ospf 10
```

VERIFICATION
```c
// VERIFICATION

show ip eigrp neighbors
show ip eigrp topology
show ip route eigrp                 
show ip eigrp interfaces
```

### OSPF
-----
[back to top](#sections)

CONFIG:
```c
// CONFIGS
// PID -> used to distinguish between multiple OSPF processes running on the same router
router ospf <PID -> Process ID>
// network statements
network 10.1.1.1 0.0.0.255 area 0
network 10.2.2.2 0.0.0.3 area 1
network 10.1.2.3 0.0.0.0 area 1
// Sets all ports the passive (not sending hello)
passive-interface default
// we are explicitly stating which we do want to send hello messages
no passive-interface <port #>
// Redistribute connected routes into OSPF
redistribute connected subnets
// Redistribute EIGRP AS 75 into OSPF (75 is just and example number)
redistribute eigrp 75 subnets
// only needed on main router that leaves the network
default-information originate    // Shares default route to other routers.
```

VERIFICATION:
```c
// VERIFICATION

show ip ospf neighbor             // shows neighbor status.
show ip ospf interfaces brief     // show interfaces matching network statements
show ip ospf rib                  // rib = routing information base

```

## BGP
----
[back to top](#sections)
```c
router bgp 1xx  // instance number
// Remote-as -> autonomous system number.
neighbor 63.2.xx.17 remote-as 7922      // Making neigbor relationship with ISP
network 63.2.xx.0 mask 255.255.255.0    // bgp uses subnet mask as opposed to wildcard 

//------------------------------------------------------------------------------------
// This is injecting the summary into BGP
ip route 10.1.1.0 255.255.255.0 Null0 name Summary_Null_Route 

// Create first condition
ip prefix-list <NAME1_OUT> seq 5 permit 10.1.1.0/24 
route-map <NAME1_OUT> permit 10          // Create a route-map
match ip address prefix-list <NAME1_OUT> // bind the prefix to the route map

// Create second condition.
ip prefix-list <NAME2_IN> seq 5 permit 10.2.2.0/24  
route-map <NAME2_IN> permit 10          // Create a route-map
match ip address prefix-list <NAME2_IN> // bind the prefix to the route map

router bgp 10 
redistribute static neighbor 10.2.2.1 remote-as 102 
neighbor 10.2.2.1 description BGP_NEIGHBOR 
neighbor 10.2.2.1 soft-reconfiguration inbound  // Essential for Filter Verifications

// dictates what gets advertised to neighbor
// neighbor <next hop> route-map <MapName> in/out
neighbor 10.2.2.1 route-map <NAME1_OUT> out     
neighbor 10.2.2.1 route-map <NAME2_IN> in
```

VERIFICATIONS:
```c
show ip nat tanslations
show run | section nat
show run | sec bgp
show ip bgp sum         // short for summery
show ip bgp neighbors
show ip route

// Reset peer to peer connections/ will refind its neighbors
clear ip bgp * soft 

// Must have "soft-reconfiguration inbound" set for peer set for peer indie router bgp // 2XX conf.
// This command stores all unfiltered routes from neighbor in seperate database
router bgp XXX 
neighbor <next hop IP> soft-reconfiguration inbound

// To view routes recieved from neighbor:
show ip bgp neighbor [x.x.x.x] received-routes

// To view routes being sent to neighbor:
show ip bgp neighbor [x.x.x.x] advertised-routes
```


### Access-List (ACL)
---- 
[back to top](#sections)

##### STANDARD ACL type Configuration:
To define a standard ACL:
```c
// WILD CARD SUBNET NOTATION
access-list <10> permit <10.0.0.0> <0.255.255.255>
access-list <10> <permit/deny> host <3.3.3.3>
```

To apply standard ACL to interface for filtering:
```c 
interface <g0/0>
ip access-group <10> <in/out>
```

##### EXTENDED ACL type Configuration:
To define an extended ACL:
```c
// WILD CARD SUBNET NOTATION
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

VERIFICATIONS:
```c
// show all ACL entries
show access-lists

// List a specific ACL entry
show access-lists 10    // seq number

// Check if ACL applied to interface
show ip interface g0/0
```


### Prefix Lists
----
[back to top](#sections)

CONFIGURE:
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

VERIFICATION:
```c
show ip prefix-list                           // To list all prefix lists:
show ip prefix-list MyPrefixList              // MyPreFixeList is the name of list
show running-config | section ip prefix-list  // show configured lists.
```


### Route Maps
-----
[back to top](#sections)
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

VERFICATIONS:

```c
show route-map
show route-map MyRouteMap 
```

## NAT/PAT
----
[back to top](#sections)

PAT (OVERLOAD)
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

Reverse NAT setup
```c
// OUTSIDE is case sensitive and is just a label for the internet.
ip nat pool OUTSIDE 10.2.12.249 10.2.12.254 netmask 255.255.255.248

ip nat outside source list 1 pool OUTSIDE add-route  
```

##### 
VERIFICATIONS
```c
show ip nat translations
show ip nat statistics
```

## HSRP
----
[back to top](#sections)

CONFIGS:
Switch 1 SVI
```c
// STANDBY NOTE:
//------------------------
// The standby number is arbitrary but by convention you should assign it the vlan_id 
// that it corresponds with

// Master L3 switch/router
interface vlan <XX>
ip address <vlan IP> <subnet mask>
standby version 2    // 2 is the most common
standby <Corresponding_Vlan_id> ip <virtual ip (VIP)>
standby <Corresponding_Vlan_id> priority <higher the most priority>
standby <Corresponding_Vlan_id> preempt  // This is our master switch and should have a higher priority

// slave L3 switch/router
interface vlan <XX>
ip address <vlan IP> <subnet mask>
standby version 2    // 2 is the most common
standby <Corresponding_Vlan_id> ip <virtual ip (VIP)>
standby <Corresponding_Vlan_id> priority <Needs to be a lower priority>
// NO PREEMPT ON SLAVE
```

VERIFICATIONS:
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

## DHCP
-----
[back to top](#sections)

CONFIGS:
Enable the DHCP service:
```c
service dhcp // this is placed on the device.
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

VERIFICATIONS:
To list current DHCP leased addresses:
```c
show ip dhcp binding
```

To list configured DHCP pools:
```c
show ip dhcp pool
```



### ALL VERIFICATIONS
-----
[back to tophow(#sections)

VERIFICATIONS:
 ```c
    //Layer 2 Verification Commands
    show interfaces
    show interfaces status
    show mac address-table
    show interface switchport
    show interface trunk
    show arp
    show ip dhcp snooping binding
    show ip arp inspection
    show errdisable recovery
    show spanning-tree
    show cdp neighbors
    show vlan brief
    show int irb // itegrated routing and bridging
    
    //Layer 3 Verification Commands
    show interface
    show ip interface brief
    show ip route
    show ip arp
    show ip protocols

	// BGP
	show ip bgp
	show ip bgp neighbors
    show ip bgp summary
    show ip routes
	show run | sec bgp
	// BGP FILTERING VERIFICATION
	// To view routes recieved Before filter:
	show ip bgp neighbor [x.x.x.x] received-routes
	// TO view routes after filter
	show ip bgp neighbor [x.x.x.x] routes
	// To view routes being sent to peer after filtering
	show ip bgp neighbor [x.x.x.x] advertised-routes
    
    // OSPF
    show ip ospf
    show ip ospf interface | include Passive
    show ip ospf neighbor             // shows neighbor status.
	show ip ospf interfaces brief     // show interfaces matching network statements
	show ip ospf rib                  // rib = routing information base
    
    // EIGRP
    show ip eigrp topology
    show ip eigrp neighbors
	show ip route eigrp
	show ip eigrp interfaces

	// Trouble shooting 
    traceroute
    ping

	// Port Channel
	show etherchannel summary

	// NAT
	show ip nat translations
	show ip nat statistics
	show run | section nat

	// DHCP
	show ip dhcp binding           // To list current DHCP leased addresses
	show ip dhcp pool              // To list configured DHCP pools

	// HSRP
	show standby brief             // easy to read summary
	show standby                   // much more detailed output
	show standby vlan <vlan ID>    // list HSRP group configured for an interface

	// Access List
	show access-lists              // show all ACL entries
	show access-lists <seq #>      // List a specific ACL entry 
	show ip interface g0/0         // Check if ACL applied to interface

	// route maps
	show route-map                 // all maps
	show route-map <ListName>      // Specific

	// Prefix lists
	show ip prefix-list            // all lists
	show ip prefix-list <ListName> // Specific
	show running-config | section ip prefix-list   // Show configured lists
	
	


```

# Network Command Comparison
----
[back to top](#sections)
```c
// Display information about network interfaces, including IP addresses, netmasks, and MAC addresses
Cisco IOS: show interfaces
Linux: ifconfig
Windows: Get-NetAdapter

// Show and modify IP addresses, routes, and other network configuration settings
Cisco IOS: show ip interface brief
Linux: ip address show
Windows: Get-NetIPAddress, Get-NetRoute

// Send ICMP echo requests to a specified destination IP address to test network connectivity
Cisco IOS: ping <destination IP address>
Linux: ping <destination IP address>
Windows: Test-Connection <destination IP address>

// Show the path that packets take to reach a specified destination IP address, including the IP addresses of the routers along the way
Cisco IOS: traceroute <destination IP address>
Linux: traceroute <destination IP address>
Windows: tracert <destination IP address>

// Display information about active network connections, including listening ports, established connections, and network statistics
Cisco IOS: show ip sockets
Linux: netstat -a
Windows: Get-NetTCPConnection, Get-NetUDPEndpoint

// Display the ARP cache, which maps IP addresses to MAC addresses on a local network
Cisco IOS: show arp
Linux: arp -a
Windows: Get-NetNeighbor

// Show the routing table, which contains information about how to reach different network destinations
Cisco IOS: show ip route
Linux: route -n
Windows: Get

// A DNS lookup utility that can be used to query DNS servers for information about domain names and IP addresses
Cisco IOS: show ip dns
Linux: nslookup <domain name>
Windows: Resolve-DnsName <domain name>

// A versatile tool for testing network connections, including TCP and UDP connections, port scanning, and file transfers
Cisco IOS: ping <destination IP address> source <source IP address>
Linux: nc -vz <destination IP address> <port number>
Windows: Test-NetConnection <destination IP address> -Port <port number>

// Display information about the network interfaces on a device, including their status and configuration
Cisco IOS: show interfaces status
Linux: ip link show
Windows: Get-NetAdapter

// Display information about the wireless network interfaces on a device, including their status and configuration
Cisco IOS: show interfaces dot11Radio
Linux: iwconfig
Windows: Get-NetAdapter -Wireless

// Display information about the DHCP leases assigned to a device
Cisco IOS: show ip dhcp binding
Linux: dhcpd-pools -c /etc/dhcp/dhcpd.conf
Windows: Get-DhcpServerv4Lease
```


# Linux
-----
[back to top](#sections)
```sh
# Display information about network interfaces, including IP addresses, netmasks, and MAC addresses
ifconfig

# Show and modify IP addresses, routes, and other network configuration settings
ip address showip 
ip route show

# Send ICMP echo requests to a specified destination IP address to test network connectivity
ping <destination IP address>

# Show the path that packets take to reach a specified destination IP address, including the IP addresses of the routers along the way
traceroute <destination IP address>

# Display information about active network connections, including listening ports, established connections, and network statistics
netstat -a

# Display the ARP cache, which maps IP addresses to MAC addresses on a local network
arp -a

# Show the routing table, which contains information about how to reach different network destinations
route -n

# A DNS lookup utility that can be used to query DNS servers for information about domain names and IP addresses
dig <domain name>

# A more modern replacement for netstat that displays information about active network connections and sockets
ss -a

# A versatile tool for testing network connections, including TCP and UDP connections, port scanning, and file transfers
nc -zv <destination IP address> <port number>
```

# Windows
-----
[back to top](#sections)
```powershell
# Display information about network interfaces, including IP addresses, netmasks, and MAC addresses
Get-NetAdapter

# Show and modify IP addresses, routes, and other network configuration settings
Get-NetIPAddress
Get-NetRoute

# Send ICMP echo requests to a specified destination IP address to test network connectivity
Test-Connection <destination IP address>

# Show the path that packets take to reach a specified destination IP address, including the IP addresses of the routers along the way
tracert <destination IP address>

# Display information about active network connections, including listening ports, established connections, and network statistics
Get-NetTCPConnection
Get-NetUDPEndpoint

# Display the ARP cache, which maps IP addresses to MAC addresses on a local network
Get-NetNeighbor

# Show the routing table, which contains information about how to reach different network destinations
Get-NetRoute

# A DNS lookup utility that can be used to query DNS servers for information about domain names and IP addresses
Resolve-DnsName <domain name>

# A versatile tool for testing network connections, including TCP and UDP connections, port scanning, and file transfers
Test-NetConnection <destination IP address> -Port <port number>
```


