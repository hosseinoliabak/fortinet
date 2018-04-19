# AntiVirus and Conserve Mode
### Malware 
* Virus: propagated through physical media and then activated  
* Worm: instead of media, it uses network to propagate 
* Trojan: a program looks like nice but is evil; does not propagate other than copying 
  * Remote Access Trojans (RATs): manually from remote 
* Ransomware/Crypto-Malware: locks your data 
* Logic Bomb: a set of instructions secretly incorporated into a program so that if a particular condition is satisfied they will be carried out, usually with harmful effects; for example, when I was fired 
* Rootkit: grabs administrative privileges to do many stuff on the computer 
* Backdoor: A programmer may sometimes install a back door so that the program can be accessed for troubleshooting or other purposes. However, attackers often use back doors that they detect or install themselves, as part of an exploit 
* Polymorphic Malware: changes itself to confuse the digital signature of anti-malware program 
* Armored Virus: protect itself from antivirus programs, making it more difficult to trace 
* Keylogger: records key strokes 
* Grayware: annoying; may cause security risks
  * Spyware: tracking all kind of stuffs 
  * Adware: pops up annoying adds, but is really harmful 

### Evasion techniques
* Encryption: Payload is encrypted and a decryption function that is included in the code is used before executing
* Polymorphism: Uses encryption keys or can changes the code when executing, but the function (definition) of the code does not change
* Metamorphism: Rewrites its own code which looks totally different with each infection Used to avoid pattern-recognition

### Antivirus
* Database if virus signatures to identify infections
* Virus names: `<vector>/<pattern>`: Example: `W32/Kryptkit.EMT!tr`
  * `<vector>` for all virus will always be the same, but vendors assign different IDs for `<pattern>`
* To detect a virus, the AV engine must match file with pattern (`<signature>`)
* Each vendor uses different detection engines & signatures
  * MD5
  * CRC
  * Combination of file attributes
  * Binary values in some areas
  * Encryption keys
  * Parts of code
* FortiGate AV scanning techniques:
  1. Antivirus Scan: Real-time
  2. Grayware Scan (Optional): Must be enabled in CLI; AV actions apply
  3. Heuristics scan (Optional): Must be enabled in CLI; looks for virus-like code; Can detect zero-day viruses (Viruses that exist but have yet no signature);
  false positive possible
 
#### FortiGate Inspection Modes
* Proxy-based: Supports more features - DNS filter, Explicit Proxy, WAF ...
  * Per VDOM: default mode is proxy-based
* Flow-based: Optimized for performance

### Memory Conserve Mode
* FortiOS protects itself when memory usage is high to prevent using so much memory that the FortiGate becomes unresponsive.
One usage is lower, FortiGate leaves conserve mode. There are 2 types of memory conserve mode:
  * Kernel
  * System