# Introduction
FortiGate Security 5.4.2

## Why FortiGate?

**Unified Threat management (UTM):**

Single security appliance, that provides multiple security functions at a single point on the network.
For smaller to mid-size businesses or enterprise branch offices, unfied threat management is often a superior solution,
compared to separate dedicated appliances.

**Platform Design:**

![fortigate platform design](https://user-images.githubusercontent.com/31813625/38750152-0c91ae36-3f22-11e8-8e89-ce8a733318a5.png)

* FortiGuard Subscription Services
  * Internet connection and contract required
  * Provided by GortiGuard Distribution Network (FDN)
    * Major datacenters in North America, Asia, and Europe or from FDN through your FortiManager
  * Package updates: FortiGuard AV and IPS
    * update.fortiguard.net
    * TCP port 443 (SSL)
  * Live queries: FortiGuard Web Filtering, DNS Filtering, and Antispam
    * service.fortiguard.net
    * Proprietary protocol on UDP port 53 or 8888    

**FortiGate Hardware:**
* Application-specific integrated circuit (ASIC)
* CP or NP chip handles cryptography and packet forwarding 

## Network Architecture
Modes of operation
* NAT:
  * Default operation mode
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
* port1 / internal interface IP: `192.168.1.99/24`
* `PING`, `HTTP`, `HTTPS`, and `SSH` protocol management enabled
* Built-in DHCP server is enabled on port1 / internal interface
* Default username is `admin` with no password