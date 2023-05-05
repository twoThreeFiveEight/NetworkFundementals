On router Illinois and SIC-RTR-1

```c

!!! THE CONFIGURATION BETWEEN ASTON AND SIC MUST MATCH !!!!
// Router IL
// create Isakmp
crypto isakmp policy 1 
encryption aes 
authentication pre-share 
group 16        // should remain 16 -> part of the encryption algoirthm
// crypto isakmp key test address <network interface we wish to enter with tunnel>
crypto isakmp key test address 180.3.29.18
//
crypto ipsec transform-set Aston_SIC esp-aes esp-sha512-hmac
mode tunnel
//
crypto ipsec profile Aston_SIC
set transform-set Aston_SIC

// ----------------------------------------------------------------------------------
// FLIP THESE IPS FOR SIC CONFIGURATION.
// Create Logical Interface 
int tunnel0
ip address 192.168.29.2 255.255.255.0
tunnel source g0/1
tunnel mode ipsec ipv4
tunnel destination 180.3.29.18
tunnel protection ipsec profile Aston_SIC

// IN prefixes
ip prefix-list SIC_IPsecPrefixes_IN seq 10 permit 192.168.29.0/24
ip prefix-list SIC_IPsecPrefixes_IN seq 20 permit 10.3.29.0/24
// route map ipsec IN
route-map SIC_IPsecRouteMap_IN permit 10               
match ip address prefix-list SIC_IPsecPrefixes_IN

// Out prefixes
ip prefix-list ASTON_IPsecPrefixes_OUT seq 10 permit 192.168.29.0/24
ip prefix-list ASTON_IPsecPrefixes_OUT seq 20 permit 10.2.29.0/24
// route map IPsec out
route-map ASTON_IPsecRouteMap_OUT permit 10              
match ip address prefix-list ASTON_IPsecPrefixes_OUT

// router bgp
router bgp 129                 // can have multiple neighbors under one AS
neighbor 180.3.29.18 remote-as 229
neighbor 180.3.29.18 ebgp-multihop 2
neighbor 180.3.29.18 soft-reconfiguration inbound
neighbor 180.3.29.18 route-map SIC_IPsecRouteMap_IN in
neighbor 180.3.29.18 route-map ASTON_IPsecRouteMap_OUT out 
// 
// need a static route 
ip route 10.3.29.0 255.255.255.0 tunnel0
```

VERIFICATION:
```c
trace <ip> 
show crypto ipsec sa
show crypto isakmp sa
```



