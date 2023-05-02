#### BGP CONFIG
```c
//rtrIL
router bgp 1xx  // instance number

// Remote-as -> autonomous system number.
neighbor 63.2.xx.17 remote-as 7922      // Making neigbor relationship with ISP
network 63.2.xx.0 mask 255.255.255.0    // bgp uses subnet mask as opposed to wildcard 




// inside Nat is done inside![[20230414_105217.jpg]] the interface
int g0/2 - 4   // this is a range syntax for port 2-4 works for other things as well
ip nat inside

// GLOBAL CONFIGURTATION MODE

// outside Nat is done in the global config mode
int g0/1
ip nat outside

// ACL 
access-list 1 permit 10.2.XX.0 0.0.0.255  
access-list 2 permit 180.3.XX.0 0.0.0.255
```


#### VERIFICATION COMMANDS
```c
show ip nat tanslations
show run | section nat
show ip bgp sum
show run | sec bgp
```