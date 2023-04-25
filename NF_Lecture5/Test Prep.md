### What to know?

- Know STP port roles, what their function, and how to identify them
	- root
		- lead to the root bridge
		-  shortest port cost from each individual device
	- designated
		- everything that is not root
	- blocked
		- redundant link, Not root or designated.
		- Always in port state "blocked" -> Recieves data (BPDU) but does not forward data

- Port states DIFFERENT port roles.
	- Port role is the job of the port (blocked port job) but port state is its current state.
	- port states
		- Root/designated ports transition through these states.
		- Blocked ->  Recieves data (BPDU) but does not forward data
		- listening ->listening/sending BPDUs, but forwarding data
		- learning -> listening/sending, listens and LEARNS MAC address, but does not forward data
		- FORWARDING -> Listening/ Sending BPDUs and forwards data.
			- most desirable

- Why would engineers change the STP proirity value?
	- if we have a new/ more powerful switch with a higher mac address than a older model switch that we dont want to be our RB
- 
- Know the purpose STP
	- prevent layer2 loops
	- block redundant ports/ links

- what is portfast?
	- forces port to skip blocked, listening, and learning state and move into forwarding

how do switches communicate in STP?
- BPDUs


- Know how link speed (cost) will switch the best path ipsofacto port roles

- RPVST+
	- combines listening/ blocking into "discard state"
	- per vlan splits