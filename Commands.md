
## Sections:
----
Basic:            [FindThings](#findthings) | [Modes](#modes) | [TroubleShoot](#troubleshoot) | [Section](#section) | [InterfaceRanges](#interfaceranges)

Devices:        [InitialConfig](#initialConfig) | [SwitchConfig](#switchconfig) | [L3 Switch](#l3%20switch) | [Router](#router) 

Protocols:     [EIGRP](#eigrp) | [OSPF](#ospf) | [BGP](#bgp) | [NAT/PAT](#nat/pat) |

ALL:              [ALL VERIFICATIONS](#all%20verifications)

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

traceroute
trace

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
Sections: [VLAN](#vlan) | [SVI](#svi) | [Trunk](#trunk) | [Access](#access) | [Port Channel](#port%20channel)

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

#### Access
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
```c
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

## L3 Switch
-----
[back to top](#sections)

MUST ADD THIS.
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

// LoopBack configurations
// -------------------------
interface loop0
ip address 10.2.29.194 255.255.255.255
shut
no shut
  

// Interfaces configs here
// -------------------------
// TO_ROUTER

interface g0/3
no switchport
ip address 10.2.29.201 255.255.255.252
shut
no shut


// Static Routes go here

// ospf config goes here
```


## Router
-----
[back to top](#sections)


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

router ospf <XX>
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


// inside Nat is done inside![[20230414_105217.jpg]] the interface
int g0/2 - 4   // this is a range syntax for port 2-4 works for other things as well
ip nat inside

// IF YOU WISH TO HAVE EXTERNAL ACCESS TO THE LOOP. MAKE THE LOOP `ip nat inside`
int loop0
ip nat inside

// GLOBAL CONFIGURTATION MODE

// outside Nat is done in the global config mode
int g0/1
ip nat outside

// ACL 
access-list 1 permit 10.2.XX.0 0.0.0.255  
access-list 2 permit 180.3.XX.0 0.0.0.255
```

VERIFICATIONS:
```c
show ip nat tanslations
show run | section nat
show ip bgp sum
show run | sec bgp

// Must have "soft-reconfiguration inbound" set for peer set for peer indie router bgp // 2XX conf.
// This command stores all unfiltered routes from neighbor in seperate database
router bgp XXX 
neighbor <next hop IP> soft-reconfiguration inbound

// To view routes recieved from neighbor:
show ip bgp neighbor [x.x.x.x] received-routes

// To view routes being sent to neighbor:
show ip bgp neighbor [x.x.x.x] advertised-routes
```

## NAT/PAT

CONFIG
```c

```

VERIFICATIONS
```c
show ip nat translations
show ip nat statistics
```
### ALL VERIFICATIONS
-----
[back to top](#sections)

VERIFICATIONS:
 ```c
    //Layer 2 Verification Commands
    show interfaces
    show interfaces status
    show mac-address-table
    show interface switchport
    show interface trunk
    show arp
    show ip dhcp snooping binding
    show ip arp inspection
    show errdisable recovery
    show spanning-tree
    show cdp neighbors
    show vlan brief
    
    //Layer 3 Verification Commands
    show interface
    show ip interface brief
    show ip route
    show ip arp
    show ip protocols

	// BGP
    show ip bgp summary
    
    // OSPF
    show ip ospf
    show ip ospf interface | include Passive
    
    // EIGRP
    show ip eigrp topology
    show ip eigrp neighbors
	show ip route eigrp
	show ip eigrp interfaces

	// Trouble shooting 
    traceroute
    ping
 ```
