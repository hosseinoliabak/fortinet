# Diagnostics
In order to define any problem, first you have to know what your network's normal behavior is. (baseline):
* CPU and memory usage
* Traffic volume
* Traffic direction
* Protocols and port numbers
* Traffic pattern and distribution

Getting information:
* Current status: `get system status`
* `diagnose sys top`: Child processes are listed individually
* `diagnose sys top-summary`: All child processes are listed together
* `diagnose hardware test suite all`
* `diagnose debug crashlog read`
* `get hardware nic <port>`
* `get system interface transceiver`
* `get ip arp list`
* `execute ping-options`
  * `execute ping`
* `execute traceroute`

### Debug Flow
Shows what the CPU is doing per step-by-step with the packets. If a packet is dropped, it shows the reason
* Multi-step command:
1. enable console command: `diagnose debug flow show console enable`
2. Specify the filter: `diagnose debug flow filter <filter>`
3. Enable debug output: `diagnose debug enable`
4. Start the trace: `diag debug flow trace start [number_of_packets]`
5. Stop the trace: `diagnose debug flow trace stop`