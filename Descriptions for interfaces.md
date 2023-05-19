You can add a description to every interface when configuring a network device. Adding descriptions to interfaces can be helpful for documentation and troubleshooting purposes. Here's an example of how to add a description to an interface in Cisco IOS:

```c
interface GigabitEthernet0/0
 description This is the interface connected to the LAN
 ip address 192.168.1.1 255.255.255.0
```

In this example, we add a description to the GigabitEthernet0/0 interface with the text "This is the interface connected to the LAN". You can add a description to any interface on a network device, including physical interfaces, subinterfaces, and virtual interfaces. The exact syntax for adding a description may vary depending on the specific device and operating system you are using.