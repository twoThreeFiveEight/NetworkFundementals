### Tables in OSPF
- ## Neighbor table
	- Only has information on the neighbor relationships DIRECTLY CONNECTED
	- Contains IP addresses + their resepective port numbers associated to its neighbor
	- Contains "State" of the relationship. 
```c
show ip ospf neighbor
```

- ## Topology table
	- Contains everything OSPF knows about. 
	- Link State Datebase (LSDB)
		- Each entry in the LSDB is known as: Link State Advertisement (LSA )
```c
show ip ospf database
```

##### DATABASE TABLE WITH LSA's:
-----
Router Link States AREA 0:
| | Link ID | ADV Router | Age | Seq #| Checksum | links |
|----|---|---|---|---|---|----|
|LSA|1.1.1.1|1.1.1.1|783|0x80000005|ox00FAEB|7|
|LSA|2.2.2.2|2.2.2.2|748|0x80000005|0x0031A9|9|
|LSA|3.3.3.3|3.3.3.3|727|0x80000004|0x00701F|7|
|LSA|4.4.4.4|4.4.4.4|746|0x80000003|0x00240B|8|
|LSA|5.5.5.5|5.5.5.5|739|0x80000003|0x00C036|2|
Net Link States AREA 0:
| | Link ID | ADV Router | Age | Seq #| Checksum | 
|----|---|---|---|---|---|
|LSA|10.0.1.1|1.1.1.1|783|0x80000004|ox0039D1|
|LSA|10.0.2.2|2.2.2.2|748|0x80000004|0x00F50F|
|LSA|10.0.3.3|3.3.3.3|727|0x80000003|0x00A45B|
|LSA|10.0.4.4|4.4.4.4|746|0x80000003|0x00B433|
|LSA|10.0.5.5|5.5.5.5|739|0x80000003|0x00A234|

----

- ## Routing table
	- Table router use to route.
	- ONLY CONTAINS BEST ROUTES from the topology table
```c 
show ip route
```


## OSPF Packets
-----
HELLO packets are constantly being sent out to discover new OSPF relationships. These are sent via IP address 244.0.0.5. IF and ONLY IF a router discovers a new router is will send a DBD packet which is a SUMMARY of the LSDB (entire topology table) not the whole thing. This spares the router they jsut discoverd from the computational weight of recieving the entire database. The recieving router then parses the DBD and will only request the LSAs that is does not yet contain. This request for the missing LSAs is the LSR. The original sending router will then send a LSU that will contain all the LSAs the requesting router did not yet know about. The router receiving the LSU will then send a LASck to acknowledge the LSU was received.

#### 1. Hello Packets
- Periodically sent to 244.0.0.5
	- Multicast address for ALL OSPF Routers
	- Meaning all OSPF Routers will use the same IP 244.0.0.5 to establish connection.
-   Discover other OSPF routers
- Includes info about sending Router
	- Determines wether Adjacency will form

#### 2. Database Descriptor (DBD)
- Summary of LSAs in each Router's LSDP
	- Only a summary not the entire topology.
- Avoids sending full LSDB for each Neighbor

#### 3. Link State Request (LSR)
- Sent to request full LSAs
- only if there are missing entries found during the parsing of the DBD.

#### 4. Link State Update (LSU)
- Includes requested LSAs

#### 5. Link State Acknowledgement (LSAck)
- Sent to confirm reception of LSU



