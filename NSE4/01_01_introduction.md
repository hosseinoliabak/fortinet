# Introduction

## Network Architecture
Modes of operation
* NAT:
  * FortiGate is an OSI Layer 3 **router**
  * Interfaces have IP addresses
  * Packets are routed by IP
  * NAT mode is the most common choice and is the default operation mode
  * Destination address is the FortiGate address
  * It will then apply NAT or port forwarding 
* Transparent:
  * FortiGate is an OSI Layer 2 **switch** or **bridge**
  * Interfaces do *not* have IPs
  * Cannot route packets, only forward or not
  * The destination address is the server's address
  
Factory Default Settings
* port1 / internal interface IP: 192.168.1.99/24
* PING, HTTP, HTTPS, and SSH protocol management enabled
* Built-in DHCP server is enabled on port1 / internal interface
* Default username is `admin` with no password  