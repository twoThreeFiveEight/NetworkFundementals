
----
### what is new?
- default route
	- This is a route that matches all routes
		- notation: 0.0.0.0/0

- Longest match rule
	- Alway chooses the best route with the longest match.
		- This is pertaining to the subnet mask. With the "longest match" the route will prefer to travel a path that has more turned "on" bits. A higher CIDR will be defined as a more defined route.
			- If you have a subnet of /29 this is considered more defined as a /28 route. The path traveled will prefer traveling over /29

#### CA Configurations info for Scenario 2:

```c

// ! rtrCA
//-----------------------------------------------------------------
// ! static routes for california router using default route.
// ip route <network address> <subnet mask> <Next hop interface IP>
ip route 0.0.0.0 0.0.0.0 10.2.xx.70


// ! Illinios layer 3 switch
//-----------------------------------------------------------------
// ! first command needs to make the switch become a layer3 device
ip routing

// ! Static route for Illinois
// ip route <network address> <subnet mask> <Next hop interface IP>
ip route 0.0.0.0 0.0.0.0 10.2.xx.202

// ! The no switchport tells the switch the port is meant for routing not switching.
//   This needs to be declared before assigning that port a IP Address
int g0/3
no switchport // needs to be declared before adding IP to port
ip address 10.2.XX.201 255.255.255.252

// ! rtrIL
//------------------------------------------------------------------
int g0/1
ip address 63.2.XX.18 255.255.255.0

ip route 0.0.0.0 0.0.0.0 63.2.XX.17

// ! Comcast
//------------------------------------------------------------------
int loop0
ip address 8.8.8.8 255.255.255.255

int g0/1
ip address 63.2.XX.17 255.255.255.0

// ! rtrNY
//------------------------------------------------------------------
ip route 0.0.0.0 0.0.0.0 10.2.XX.74
```

## Gotchya's 
---- 
### Question: 
What do the VLANS of a layer 3 switch represent? 

##### Answer:
They are the default gateway IP's.
- Layer 3 switch -> default gateway
- Layer 2 switch -> remote managment