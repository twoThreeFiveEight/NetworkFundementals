### QUESTION: do vlans also limit collsion domains 

#### ANSWER:
Yes, VLANs (Virtual Local Area Networks) limit the collision domain because they create separate broadcast domains within a single physical network. By dividing a network into multiple VLANs, broadcast traffic can be contained within each VLAN, which helps to enhance network performance and reduce unnecessary network traffic. In other words, devices within the same VLAN will receive all broadcast messages sent within that VLAN and will not receive broadcast messages from devices in other VLANs, which helps to reduce the chances of network collisions.

#### QUESTION:
   is the reason a vlan is 0-4096 because the code behind a vlan using a "union" and 12 bits to create a protocol bits data bits and parity bits in one 12 bit sequence? 
   
#### ANSWER:
   No, the reason that a VLAN is numbered between 0 and 4095 in Cisco networking is because the VLAN ID is a 12-bit field in the Ethernet frame, which allows a maximum of 4096 unique VLAN IDs to be defined in the standard. This is a design decision and standardization by the IEEE 802.1Q specification that specifies the VLAN tagging protocol for Ethernet frames. The VLAN ID field in the Ethernet frame header consists of 3 bits for the user priority value (which is used for Quality of Service), 1 bit to indicate if the frame is a tagged or untagged frame, and 12 bits for the VLAN ID. The VLAN ID is used to identify the VLAN of the frame, and allows switches and other network devices to route the frame to the appropriate VLAN based on the VLAN ID. The VLAN ID must be unique for each VLAN in the network and in the range of 1 to 4094, with VLAN 0 and 4095 reserved for special purposes.
    
#### QUESTION:
How long is the ethernet frame and in order what sections of the frame is deticated to what information 

#### ANSWER:
The size of an Ethernet frame depends on the type of Ethernet being used. The most commonly used Ethernet standard today is Gigabit Ethernet, which has a maximum frame size of 1,518 bytes. The frame size for other Ethernet types, such as Fast Ethernet and 10-Gigabit Ethernet, are slightly different. An Ethernet frame consists of several fields, each with a specific purpose. Here are the main fields of an Ethernet frame, in order: 
    1. **Preamble and Start Frame Delimiter (SFD)** – A 7-byte sequence consisting of alternating 0s and 1s, followed by the SFD byte which indicates the start of the frame. 
    2. **Destination MAC Address** – A 6-byte field that identifies the intended recipient of the frame. 
    3. **Source MAC Address** – A 6-byte field that identifies the sender of the frame. 
    4. **EtherType/Length** – A 2-byte field that indicates the type of protocol (such as IPv4, IPv6, or ARP) carried in the payload of the frame. If the protocol type is less than 1,536 bytes (0x0600 in hexadecimal), this field is used to store the length of the payload. 
    5. **Payload** – This is the actual data being carried by the frame, such as an IP packet or an ARP request. The maximum length of the payload is 1,500 bytes. 
    6. **Frame Check Sequence (FCS)** – A 4-byte field used to detect errors in the frame, such as transmission errors or collisions. Note that some fields of the Ethernet frame size may be variable. For example, the length of the payload can vary based on the protocol used.