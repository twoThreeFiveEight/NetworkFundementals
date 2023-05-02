
### preliminary steps to narrow down issue:
----
1. Who is reporting the issue.
	1. Isolate the area effect
		1. Ideally down from the network to the subnet to the VLAN to the device.
2. When did the issue start.
	1. did something break or was it a new implimitation of some device that broke it.
3. Did this ever work?
4. is this intermitant and/or on going?
	1. A random issue might  suggest application/server-side complications, dynamic routing issues (i.e. it works every other time,  etc.), link flapping, etc. whereas a repeatable issue might suggest outright routing issues, link failure(s),  hardware failure(s), etc.

5. determine steps taken to resolve the issue
6. Prioritize hard downs over issues people can work around.

### Gather Information.
----
1.  Get at least this information from source of issue
- Source (IP address, username, hostname, application name, etc.)  
- Destination (IP address, username, hostname, application name, etc.)  
- Service/Port (HTTP, HTTPS, SSH, etc.)  
- Symptom (Can’t access, slow access, etc.)  
- Applicable error messages/logs  
- Severity  
- Duration (when did it start and is it ongoing)  
- Any troubleshooting steps already taken  
- Repeatable or random  
- User available to test

1. Find someone to help you test for the issue.
2. NEVER MAKE UNAUTHORIZED CHANGES OUTSIDE OF YOUR CUSTOMER’S CHANGE PROCESS, but if we  are able to make changes to attempt to resolve an issue it is always more convenient to get immediate feedback on whether or not the issue still persists once changes are made.

### Diagnostic steps
----
##### Identify the Traffic Path (Trace Down the Hosts)
- The two most critical pieces of information required to effectively troubleshoot an issue are the source(s) and destination(s). So, assuming we have those (which we  should), we will need to trace out where to focus our efforts; physically and/or logically. Let’s figure out  how to trace down a server or user.

#####  Given that we have the source IP and the Destination IP.

### 1. First we would trace to the source IP from a command prompt on our local PC:
```c 
// tracert = trace route
tracert 10.1.251.61            // trace from source IP
```
![[Pasted image 20230502103354.png]]
- we are interested in the second-to-last hop (the last hop being the actual  destination), because that represents the source’s gateway. 

### 2. Now, let’s access the highlighted IP  (via telnet or ssh) and login
```c
// SSH or Telnet into device.
telnet 10.1.142.4
```
![[Pasted image 20230502103520.png]]

### 3. do a `show arp | i IP of destination` to find the mac-address of the destination.
![[Pasted image 20230502104644.png]]

### 4. `show  mac address-table | i mac address of destination` to find the interface we are learning  this IP from:

![[Pasted image 20230502104705.png]]

Per the above, we now know a few things. First, we know that the source IP is on Vlan251.  
Second, we know that the gateway for this IP is on “pmlsMN010a02a16.” Third, we know that  
pmls (Device in data center) is “learning” of this host or user from Port-Channel 10 (Po10)

### 5. Let’s figure out what pmls is “learning” of this host or user from Port-Channel 10 (Po10)

`etherchannel summary` is a port channel verification command.

![[Pasted image 20230502110035.png]]

### 6. By running the “show etherchannel summary” command above, we can see the physical ports  associated with Po10. Now, we can find out if the device on the other side of this Po is another  Cisco network device by doing a “show cdp neighbors gi1/0/3”:

![[Pasted image 20230502130124.png]]

#### TODO -> FINISH SUMMARY OF TROUBLE SHOOTING.