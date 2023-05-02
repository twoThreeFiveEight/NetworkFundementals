##### Trouble shooting advice
- What layer is the issue
- what device is the issue happening to 
- just telnetting into something can tell you the layer 4 is up. 
	- which then can narrow it down to the issue living inside anything above the layer 4.
- Phrase -> trust but verify, 
	- get sceen shot of whole screen shot of error
		- Time out error = TCP or lower layer failing
		- DNS error
		- SSL Certificate error 

### Packet flow
1. Is the subnet I am trying to reach in my subnet?
	1. YES -> it sends arp for destination MAC
	2. NO 
		1. Checks routing table (route print in powershell) for "default route" + the "interface that route" is on and checks arp table (arp -a in powershell) 
			- IF default gateway is found the SRC IP will be built with the IP assigned to that corresponding port that attaches to the gateway and the layer 3 packet can be built.
			- IF MAC address is not inside arp table a ARP request for the default gateway will be sent. The return of from the default gateway will be in the form of a UNI CAST
			- IF the MAC address of the default gateway is found in the ARP table then L2 packet can be built
		 2. Now the router will know from the packets destination MAC address is its own it will know that the Packet is mean to be routed out. THE ROUTER STRIPS THE LAYER2 (ETHERNET HEADER) HEADER AND LOOKS FOR A NEW LAYER2 HEADER (ETHERNET HEADER)

			# HE WILL LEAVE OFF HERE NEXT WEEK