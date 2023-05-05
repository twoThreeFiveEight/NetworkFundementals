Sections: [Important Note](#important%20note) | [about](#about) | [Naming Convension for prefix lists and route map](#namingconvention) |

### Important Note:
In order to use a prefix for a route-map and allow advertisements of such network, the network prefix must be present in the routing table. This is because route-maps are applied to routes in the routing table, and if the network prefix is not present in the routing table, there will be no route to apply the route-map to. If the network prefix is not present in the routing table, you may need to inject a summary route into the routing table in order to advertise the network prefix. This can be achieved by configuring a summary route that covers the network prefix and pointing it to the appropriate next-hop or exit interface. Once the network prefix is present in the routing table, you can use a route-map to control how the prefix is advertised to other routers. For example, you can use a route-map to filter out certain prefixes or to modify the attributes of the prefixes before they are advertised

## About
-----

A prefix list is a filtering mechanism used in Border Gateway Protocol (BGP) to control the routes that are advertised or received by a BGP speaker. A prefix list is essentially a list of IP address prefixes (network addresses and subnet masks) that are used to match against the prefixes in BGP routing updates. In BGP, prefix lists are typically used to filter incoming or outgoing routes based on their prefix length, which is the number of bits in the subnet mask that define the network portion of the address. Prefix lists can be configured to permit or deny routes based on their prefix length, allowing network administrators to control which routes are advertised or received by their BGP speakers. Here are some examples of different variations of prefix lists in Cisco IOS: 1. Permitting a specific prefix:

```
ip prefix-list ALLOWED-PREFIXES seq 10 permit 10.0.0.0/8
```
This prefix list permits all prefixes in the 10.0.0.0/8 network. 

2. Denying a specific prefix:

```
ip prefix-list DENIED-PREFIXES seq 10 deny 192.168.1.0/24
```
This prefix list denies all prefixes in the 192.168.1.0/24 network. 

3. Permitting prefixes of a certain length:

```
ip prefix-list PREFIX-LEN seq 10 permit 10.0.0.0/8 le 24
```
This prefix list permits all prefixes in the 10.0.0.0/8 network with a prefix length of 24 bits or less. 

4. Denying prefixes of a certain length:

```
ip prefix-list PREFIX-LEN seq 20 deny 10.0.0.0/8 ge 24
```
This prefix list denies all prefixes in the 10.0.0.0/8 network with a prefix length of 24 bits or more. 

5. Permitting a specific prefix and denying all others:

```
ip prefix-list ALLOWED-PREFIX seq 10 permit 10.0.0.0/8
ip prefix-list ALLOWED-PREFIX seq 20 deny any
```
This prefix list permits all prefixes in the 10.0.0.0/8 network and denies all other prefixes. 

6. Permitting prefixes from multiple networks:

```
ip prefix-list ALLOWED-PREFIXES seq 10 permit 10.0.0.0/8
ip prefix-list ALLOWED-PREFIXES seq 20 permit 192.168.0.0/16
```
This prefix list permits all prefixes in the 10.0.0.0/8 and 192.168.0.0/16 networks. These are just a few examples of the different variations of prefix lists that can be used in BGP filtering. The specific configuration will depend on the network topology and the desired routing policies

----
``
` Match exact prefixes what is it meant by "match exact prefixes `

When we say "match exact prefixes" in the context of prefix lists, it means that the prefix list is configured to match only the exact prefix specified in the list. For example, if we have a prefix list with the entry "10.0.0.0/8", it will only match the prefix "10.0.0.0/8" and not any other prefix within the 10.0.0.0/8 network. This is in contrast to matching prefixes based on a range of prefix lengths or based on a specific network. Matching exact prefixes can be useful in BGP filtering when we want to be very specific about which prefixes are allowed or denied in the network.

`show me what another prefix within that network would be`


if we take the example of the network "10.0.0.0/8", another prefix within that network could be "10.1.0.0/16". While both of these prefixes fall within the same network range of 10.0.0.0/8, they are distinct prefixes with different subnet masks and therefore would not be matched by a prefix list entry that only specifies the exact prefix "10.0.0.0/8".


## NamingConvention
----

in Cisco IOS, a sequence number is used to identify the order in which statements are processed in a route map. Each statement in a route map is assigned a unique sequence number, which determines the order in which the statements are evaluated. 

In the example I provided earlier, the route map "ASTON_In" has four permit statements numbered 10, 20, 30, and 40. These statements are evaluated in the order of their sequence numbers, with statement 10 being evaluated first, followed by statement 20, then 30, and finally 40. 

The name variable for a prefix list and a route map is simply a user-defined name that is used to identify the list or map. The name variable is used when configuring the list or map, and it is also used when referring to the list or map in other configuration commands. 

For example, in the syntax I provided earlier, the prefix list is named "ASTON_In_Prefixes" and the route map is named "ASTON_In". These names are used throughout the configuration to refer to the prefix list and route map respectively.

In Cisco IOS, you are allowed to use the same variable name for each prefix list as long as the sequence number is different. This means that you can create multiple prefix lists with the same name, as long as each list has a unique sequence number. 

For example, let's say you want to create two prefix lists, each with two prefixes. You could use the same variable name for both lists, but you would need to assign a different sequence number to each list. Here's an example: 

```c
ip prefix-list ASTON_In_Prefixes seq 10 permit 192.0.2.0/24 
ip prefix-list ASTON_In_Prefixes seq 20 permit 198.51.100.0/24 
ip prefix-list ASTON_Out_Prefixes seq 30 permit 203.0.113.0/24
ip prefix-list ASTON_Out_Prefixes seq 40 permit 2001:db8::/32 
```

In this example, we are creating two prefix lists named "ASTON_In_Prefixes" and "ASTON_Out_Prefixes". Each list has two prefixes, and each prefix is assigned a sequence number. The "ASTON_In_Prefixes" list has sequence numbers 10 and 20, while the "ASTON_Out_Prefixes" list has sequence numbers 30 and 40. 

You can then use these prefix lists in a route map, as I showed in my earlier example. When configuring the route map, you would refer to the prefix lists by their variable names and sequence numbers. For example, let's say you want to use the "ASTON_In_Prefixes" list in a route map. You could do so like this:

```c
route-map ASTON_In permit 10
match ip address prefix-list ASTON_In_Prefixes
```

In this example, we are creating a route map named "ASTON_In" and assigning it sequence number 10. We then use the "match ip address prefix-list" command to specify that we want to match packets that match the prefixes in the "ASTON_In_Prefixes" list. Similarly, if you wanted to use the "ASTON_Out_Prefixes" list in a route map, you could do so like this:

```c
route-map ASTON_Out permit 10
match ip address prefix-list ASTON_Out_Prefixes
```

In this example, we are creating a route map named "ASTON_Out" and assigning it sequence number 10. We then use the "match ip address prefix-list" command to specify that we want to match packets that match the prefixes in the "ASTON_Out_Prefixes" list. By using prefix lists in route maps, you can control the flow of traffic in your network and implement policies that meet your organization's needs.

### Example 2:
If you have multiple prefix lists with the same name, you can refer to them using the same variable name in a route map. In your example, if you have five prefix lists named "MyList" with sequence numbers 10, 20, 30, 40, and 50, you can use the following configuration to match all five lists in a route map:

```
route-map MapContainingFivePrefixes permit 10
match ip address prefix-list MyList
```

This configuration will match all prefixes in all five "MyList" prefix lists, as long as the route map is applied to an interface or neighbor.