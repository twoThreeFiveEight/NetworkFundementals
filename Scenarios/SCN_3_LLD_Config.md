What is new?
- Dynamic routing protocols
	- OSPF
		- sharing routes between same type of routing protocol e.g. ospf -> ospf is called route distribution.
		- sharing routes between different types of routing protocols e.g ospf -> eigrp is called mutual redistribution.
	- EIGRP
		- sharing routes between same type of routing protocol e.g. ospf -> ospf is called route distribution.
		- sharing routes between different types of routing protocols e.g ospf -> eigrp is called mutual redistribution.

```c
// rtrIL029a02a1
// OSPF configurations 
// Process ID's PID
router ospf 20
network 10.2.29.200 0.0.0.3 area 0  // network statement, 0.0.0.3 is wild card mask 
network 10.2.29.68 0.0.0.3 area 1   // network statement
network 10.2.29.72 0.0.0.3 area 2
default-information originate       // Necessary to redistribute default route


// rtrCA029a02a01
// ospf configs
router ospf <arbitrary number>
network 10.2.29.68 0.0.0.3 area 1
passive-interface g0/3           // tells not to send hello messages down non ospf area
redistribute connected subnets
redistribute static subnets

// SIC-RTR-1
// EIGRP Configs
router eigrp <XX>
network 10.2.29.72 0.0.0.3       // No area used in eigrp
passive-interface g0/1 
redistribute static
default-metric 10000 10 255 1 1500 // K values -> Cisco says NEVER change.

```
