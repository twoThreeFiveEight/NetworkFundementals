- Filtering in BGP just defines the rule for what routes we allow out and what routes we allow in to popluate our routing table

```c
// router Filtering
1. Create prefix-list
2. Create route map
3. Add (bind) the prefix-list to the route map 
4. Apply the route map to our neighbor

// INBOUND CONFIGS
// -------------------------------
// Name for prefix list is case sensitive 
// seq = sequence number -> usually in increments in 10. 
// ip prefix-list <NameForList> seq <XX in tens> permit <what IPs we will allow in> 
// in global config 
ip prefix-list IPS_IN seq 10 permit 0.0.0.0/0
route-map ISP_IN permit 10               // create route map
match ip address prefix-list ISP_IN      // Binds prefix list to route map.

// in bgp
router bgp 129
neighbor 63.2.29.17 route-map ISP_IN in  // the "in" is a keyword

// OUTBOUND CONFIGS
//-------------------------------
// The seq number here is a different instance so can also be "10"
// in global config
ip prefix-list ISP_OUT seq 10 permit 63.2.29.18/24
route-map ISP_OUT permit 10
match ip address prefix-list ISP_OUT
// in bgp
router bgp 129
neighbor 63.2.29.17 route-map ISP_OUT out 



// -----------------------------------------------------------------------------------
// ACCESS LISTS ARE ALWAYS READ SEQUENCTIALLY -> Order Matters
// THIS PORTION IS PUT IN WHAT EVER DEVICE YOU DEEM FIT
// LESS PATH THE TRAFFIC HAS TO TRAVEL TO BE DENIED THE BETTER.
// ACL LISTS 
// -> implicit deny at the end of every access list. If not in list than drop packet
//-----------------------------

// CREATE THE LIST
// ip access-list extended/standard <NAME>
ip access-list extended SERVER_ACL  // This is the name of the list
// These are the variables in the list.
// <seqnce number> permit/deny ip <Network address> <wildcard mask> host <destination to allow or deny>
10 permit ip 10.2.29.8 0.0.0.3 host 10.2.29.220
20 deny ip any host 10.2.29.220
 // This is only checked if the host is anything but a host mentioned above
30 permit ip any any 


// applying there to the interfaces
int g0/2
ip access-group SERVER_ACL out/in
```

