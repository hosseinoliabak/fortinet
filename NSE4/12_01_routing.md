# Routing with FortiGate

Routing is how a packets are sent along a path - from point to point on the network - from source to destination.
* If the destination is on a subnet not directly connected to the router, the packet is relayed to another router that is closer
* Entries in the routing table can be configured manually, dynamically, or both
* A FortiGate in NAT mode is, among other things, and OSI Layer 3 router

## Static Routing
* Configured manually by an administrator
* Simple matching of packets to a route, based on the packet's destination IP address
* Static Routes with Named Addresses:
  * Firewall addresses with the type **IP/Netmask** can be used as destination for static routes


### Equal Cost Multi-path (ECMP)
If multiple static, OSPF, or BGP routes have the same settings for:
* Destination subnet
* Distance
* Metric
* Priority

They are **all active** and FortiGate **distributes traffic** among them (equal cost multipath)

#### ECMP Methods
* Source IP (Default): Sessions from the same source IP address use same route
* Source-destination IP: Sessions with same source and destination IP pair use same route
* Weighted: Sessions are distributed based on interface weight
* Usage (Spillover): One route is used until volume threshold is reached, then next route is used

<pre>
FG200D (root) # <b>config system settings</b>
FG100D3G13803421 (settings) # <b>set v4-ecmp-mode ?</b>
source-ip-based         Select next hop based on source IP.
weight-based            Select next hop based on weight.
usage-based             Select next hop based on usage.
source-dest-ip-based    Select next hop based on both source and destination IPs.
</pre>

### Link Health Monitor (Like IPSLA in Cisco)
Periodically sends probe packets to a server through a gateway. If FortiGate doesn't receive replies within the failover
threshold, all routes using the gateway are removed from the routing table. If standby routers available, FortiGate loads and uses
them instead of routing failover.

<pre>
FG200D (root) # <b>config system link-monitor</b>
FG200D (link-monitor) # <b>edit health_monitor_1</b>
FG200D (health_monitor_1) # <b>set srcintf port1</b>
FG200D (health_monitor_1) # <b>set gateway-ip 192.168.1.1</b>
FG200D (health_monitor_1) # <b>set protocol</b>
ping        PING link monitor.
tcp-echo    TCP echo link monitor.
udp-echo    UDP echo link monitor.
http        HTTP-GET link monitor.
twamp       TWAMP link monitor.
FG200D (health_monitor_1) # <b>set update-cascade-interface enable</b></pre>

## Policy-Based routing
* More sophisticated matching than static routes
  * Protocol
  * Source address
  * Source ports
  * Destination ports
  * ToS bits
* Manually configured
* Have precedence over routing table
* If traffic matches a PBR, FortiGate either:
  * Forwards traffic to specified outgoing interface and gateway
  * Stops policy routing and uses routing table instead

## Reverse Path Forwarding (RPF) Check 
* Protects against IP spoofing attacks
* The source IP address is checked against the routing table for reverse path
* RPF is only carried out on:
  * The first packet in the session, not on reply
  * The next packet in the oiginal direction after a route change, not on reply
* 2 modes
  * Loose RPF (Default): checks only for the existence of at least one active route back to the source using incoming interface
  * Strict: Checks that the best route back to the source uses the incoming interface. If packet is received on an interface
  that is not used to forward traffic to the source, then packet is dropped  

## Internet Services
* Database (Regularly updated via ForiGuard) that contains IP addresses, IP protocols, and port numbers used by the most common Internet services
* Used to route traffic to any well-known Internet service through specific WAN interfaces
* They are not added to the routing table, but to the policy routing table

## WAN Link Load Balancing (Replaced by SD-WAN as of FortiOS 5.6)
* A virtual WAN link consists of multiple interafces (members) connected to multiple ISP links
  * FortiGate sees all WAN load balancing members as a single logical interface
  * Simplifies the configuration
  * There can be only one WAN link load balancing group per VDOM
  * Methods:
    * Source IP
    * Source-destination IP
    * Spillover
    * Sessions
    * Volume: Sessions distributed so that traffic volume is distributed by the interface weighs 

## SD-WAN
* Virtual interface consisting of a group of member interface that can be connected to different link types
* Allows effective WAN usage with various load balancing algorithms
* Supports link quality measurement
  * Dynamic link selection based on link quality
  * Ensures HA of business critical applications

## Dynamic Routs
* Paths are automatically discovered. FortiGate talks with neighboring routers to find the best routes. Paths are also
based on the packet's destination IP address. Routing becomes somewhat self-organizing
* FortiGate supports: RIP, OSPF, BGP, ISIS 

## Routing Table Monitor:
* Displays only active routes (best paths)
* No policy routes. why? by design, Policy Routes override the routing table. So they have to be a separate table
* `get router info routing-table all`: Shows only active routes
* `get router info routing-table database`: with this command you can see both active and inactive routes


