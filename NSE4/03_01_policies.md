# Firewall Policies
Firewall policies define which traffic matches them and what FortiGate will do if it matches. Should the traffic be allowed?
If the policy does not block the traffic, FortiGate begins a more computationally-expensive security profile inspection
AKA Unified Threat Management (UTM) - such as AV, Application Control, and web filtering.

* Policies define:
  * Which traffic matches them
  * How to process traffic that matches
* When a new IP session packet arrives, FortiGate:
  * Starts at the top of the list to look for a policy matching
  * Applies the first matching policy
* Implicit deny:
  * No matching policy? FortiGate drops packet
  
How are policy matches determined?
* Incoming and outgoing interface
  * Can be interfaces(s) or a zone
  * Zone: Logical group of interfaces
* Source and destination IP address/user/device
  * Must specify at least one source address (IP address or range, subnet, FQDN, Geography) and may specify whether, neither, or both: source user / source device
    * There are 2 device identification techniques
      * With an agent (FortiClient): FortiClient sends information to FortiGate; the device is tracked by its unique FortiGate User ID (UID)
      * Agentless: FortiGate and thwe workstations are in the same subnet. devices are indexed by their MAC. To identify the device: HTTP User0Agent header, TCP fingerprint, MAC address OUI,...
  * Like source, destination criteria can use address object and also (As of FortiGate 5.6.2) Internet Service Database (ISDB) objects
    * No option to specify user or device like we had in source
    * ISDB is a database containing IP addresses, IP protocols, and port numbers
      * IF ISDB is selected: you cannot use **Address** in destination. You cannot select **service** in the firewall policy
* Services
* Schedules
* Action: ACCEPT or DENY