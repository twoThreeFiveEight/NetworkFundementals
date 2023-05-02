Lecture 2

### Q: Understand what a collision domain is and how to identify it!   
- any interface is considered a collision domain
	- in a switch network, each switch port is a seperate collision domain.
- hubs, half duplex, and shared media are all examples of collsion domains. 
- segment or portion of a network where network devices share the same network bandwith and compete for access to the media.
- collisions can occur when two or more devices try to transmit data at the same time. 

### Q: Understand how switches populate their mac address-tables - be able to provide an example!
- When a switch receives a frame from a device, the switch looks at the source MAC address of frame and associates it with the port the frame was received on. The switch looks at the MAC address table for the source MAC address and If the switch does not have an entry in its MAC address table, the Switch will map that interface to the devices MAC address.
- When a switch needs to forward the frame it will look at the MAC address table for the destination MAC address. If one is not found the Switch will broadcast the frame out of all its ports (address forwarding) (except the port it was recieved on) in a attempt to reach the destination. 
- MAC address learning only occurs in one direction. which is from the source device to the switch

A: client A sends a request to client B the frame gets sent to switch1. The Switch looks at the frame source address and references the MAC address table for the devices MAC address. If not found the switch will save the MAC address to the MAC address table. If the destination MAC is not found the switch will address forward the frame to all ports except the source port. If found the frame will be forwarded to the destination device. 

### Q: Be familiar with the "show commands" covered/provided in lecture and when to use them!  
- look up verification commands

### Q: Know the switchport modes and when you'd likely use each.  
- Access mode 
	- Connected to an end device. printer, server
	- This mode only allows traffic from a single VLAN and does not add any VLAN information to the frames. 
	- NO VLAN tags
- Trunk mode
	- Port is connected to another switch or device that supports VLANS. 
	- This port can carry traffic from multiple VLANS and adds VLAN information to the frames
	- Adds VLAN tags

### Q: Be able to provide configuration syntax.   
- look at notes

### Q: Understand what a broadcast domain is, how to identify it, and how to segment/limit it!
- A network segment that where a broadcast sent by a device is received by all devices within that segment.
	- broadcasts are used by APR (address resolution protocol), DHCP (dynamic host configuration protocol)
- Vlans can be used to seperate and limit boardcast "range" 
- subnetting divides networks into smaller subnetworks or subnets. Each subnet has its own broadcast domain.
- Router can be used to seperate the domain.

Benifits:
- increased performance
- reduction of network congestion
- increase in local bandwidth.