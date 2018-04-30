# Hardware Acceleration
With hardware acceleration, a FortiGate's CPU transfers its processing load to AICSs. The main FortiGate components are:
* Memory
* CPU
* Controllers (BUS, USB, disc)
* Integrated switch
* Network interface
* Fortinet hardware accelerators
  * Application-Specific Integrated Circuits (ASICs): FortiASICs; Not available on FortiGate VM
    * Network Processor (NP): Operates at interface, link aggregation, packet transmission, HA, IPsec phase 2 and
    hashing. `get hardware npu np6 port-list`
    * Content Processor (CP): Closer to application: Encryption/Decryption, AV, `get hardware status`
  * Security Processor (SP): Bound to interface adding additional packet processing, Multicast, Packet transmission, IPS,
  DoS, Flow-based AV. `diagnose npu spm list`
  * System on a Chip: Unifies FortiASIC-NP, FortiASIC-CP, General purpose CPU, Memories, Network interfaces