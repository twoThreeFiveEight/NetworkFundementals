Show commands

HSRP show commands
```c
// HSRP show commands
show standby bri     
show standby
```

Port Channel 
```c
// port channel
show ip int bri                 // looking for up, up state here
show etherchannel summary       // This will show summary about port channel
```

CONFIGS
```c
// IP Helper addresses are needed on each defualt gateways (SVI for L3 switches)

// California 
// IP Helper address
int g0/3.1291
ip helper-address 10.2.29.228

int g0.3.2291
ip helper-address 10.2.29.228

// L3 switches 01 in IL
// Port Channel -> Universally everyone starts with channel port 1
interface range g0/1-2
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 90,3290
switchport mode trunk
channel-group 1 mode active

interface port-channel 1
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 90,3290
switchport mode trunk

// OSPF
router ospf XX
network 10.2.29.204 0.0.0.3 area 0
network 10.2.29.196 0.0.0.3 area 0 // adding the vlan 90 for ospf port-channel routing

// HSRP
// done inside our vlan default gateways
// preempt only goes on one switch that is priority not the other.
int vlan 1290
standby version 2
standby <xx> ip 10.2.29.211       // standby <group number> ip <vlan ip> 
standby <xx> priority 110         // standby <group number> priority <higher = primary>
standby <xx> preempt              // standby <group number> preempt 

int vlan 2290
standby version 2
standby <xx> ip 10.2.29.219       // standby <group number> ip <vlan ip> 
standby <xx> priority 110         // standby <group number> priority <higher = primary>
standby <xx> preempt              // standby <group number> preempt 

int vlan 3290
standby version 2
standby <xx> ip 10.2.29.227       // standby <group number> ip <vlan ip> 
standby <xx> priority 110         // standby <group number> priority <higher = primary>
standby <xx> preempt              // standby <group number> preempt 
```