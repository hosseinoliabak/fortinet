# Firewall Policies
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
* Source and destination IP address/user/device
* Services
* Schedules
* Action: ACCEPT or DENY