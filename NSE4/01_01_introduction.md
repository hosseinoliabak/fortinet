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
  * Package updates (**Periodically**): FortiGuard AV and IPS
    * update.fortiguard.net
    * TCP port 443 (SSL)
  * Live queries (**Real-time**): FortiGuard Web Filtering, DNS Filtering, and Antispam
    * service.fortiguard.net
    * Proprietary protocol on UDP port 53 or 8888    

![image](https://user-images.githubusercontent.com/31813625/39777670-2a68d056-52d2-11e8-9a99-f8fbd8465e67.png)

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

## Administrator Methods
* CLI: Console, SSH, Telnet, GUI Widget (JavaScript)
* GUI: FortiExplorer, Web Browser

Basic CLI commands:
* `get system status` to check what the current status of FortiGate is
* `show full-configuration system interface <port>` what are all attribute values for the system interface
* `show system interface <port>` what are the non-default values for the system interface

Password strength auditing tool:
* John the Ripper: http://www.openwall.com/john/

Administrator Profiles Hierarchy
* `super_admin`: full global access
* `custom_profile1`: Partial global access
* `prof_admin`: full access in VDOM
* `custom_profile2`: partial access in VDOM

### Resetting a lost admin password
1. Hard power cycle
2. Only during first 30 seconds after boot (varies by model)
   * Tip: Copy the serial number into the terminal buffer, then paste
3. Trough the hardware console port
   * Username: `maintainer`
   * Password `bcpb`<serial number in UPPER CASE>
   * Commands:
     <pre>
     config sys global
     set admin-maintainer disable
     end</pre>

## Upgrade firmware process
1. Backup the configuration
2. Download a copy of the current firmware, in case reversion is needed
3. Have physical access, or a terminal server connected to local console, in case reversion is needed
4. Read the *Release Notes*; they include the upgarade path and other useful information
5. Perform the upgrade

## FortiGate Security Fabric
An enterprise solution that enables a holistic approach to network security, whereby the network landscape is visible
through a single console and all network devices are integrated into centrally managed and automated defence.

The security fabric has these attributes:
* Broad: covers the entire attack surface from IoT to the cloud
* Powerful: Security processors
* Automated: Threat intelligence; real time; automated response to threats
* Open: the API allows for 3rd party device integration

Devices that comprise the security fabric:
* Core - must have
  * 2 or more FortiGate devices + FortiAnalyzer
* Recommended - adds significant visibility or control
  * FortiManager, FortiAP, FortiSwitch, FortiClient, FortiSandbox, FortiMail
* Extended - Integrates with fabric, but may not apply to everyone
  * Other Fortinet products and 3rd party products using the API  