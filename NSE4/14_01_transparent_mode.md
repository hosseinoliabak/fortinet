# Transparent mode and layer 2 switching

## Virtual Wire Pairing
* Logically links 2 physical interfaces: Usually one Internal and one external interface
* Traffic is captured between these interfaces
  * Incoming traffic to one interface is always forwarded out through the other interface
  * No other traffic can enter or leave a port pair
* Avoids complexities such as broadcast storm, MAC flapping  
* Wildcard VLAN:
  * Enable: Policies applies equally to the physical interfaces and VLANs 
  * Disable: Policies applies only to the physical interfaces (packets with VLAN tags are denied)

## Software switch:
* Groups multiple interfaces into a single virtual interface
* Only supported in NAT mode VDOMs
* Acts like a hardware layer 2 switch
  * The interfaces belong to the same broadcast domain and share the same IP address