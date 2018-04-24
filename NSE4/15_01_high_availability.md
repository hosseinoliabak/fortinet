# High Availability (HA)
* Primary: Active
* Secondary: Standby whose **configuration** is being synchronized by HA primary

How about throughput?
HA Modes:
* Active-Passive: Only the primary processes traffic
* Active-Active: Primary load balances sessions for cluster

### FortiGate Clustering Protocol (FGCP)
Cluster uses FortiGate clustering protocol (FGCP) to:
* Discover other FortiGates that belong to the same HA group
* Elect the primary
* Synchronize configuration and other data
* Detect when a FortiGate fails

Runs only over the heartbeat links
* TCP 703 with different ethernet type values:
  * 0x8890: NAT mode
  * 0x8891: Transparent mode
* TCP 23 with Ethernet type 0x8893 for configuration synchronization

### HA requires
* Two to four identical FortiGates must have the same
  * Firmware
  * Hardware model and VM license
  * Hard drive capacity and partitions
  * Operating mode (NAT ot transparent)
* One link (preferably 2 or more up to 8 links) between FortiGates for heartbeat
* Same interfaces in each FortiGate connected to the same broadcast domain
* Since FortiOS 5.2, DHCP and PPPoE interfaces are supported

### Primary FortiGate election
* Override disabled (default): Negotiate begins, primary FortiGate is chosen with the greater
  1. Monitored ports whose status is up
  2. System uptime
  3. Priority
  4. Comparing serial numbers
* Force a failover
  * Change the HA priority