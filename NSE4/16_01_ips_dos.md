# Intrusion Prevention System

* Anomaly:
  * Can be zero-day or denial of service (DoS) attack
  * detected by behavioral analysis: Rated-based IPS signatures; DoS policies; Protocol constrains inspection
  * Examples: Abnormally high rate traffic (DoS/flood)
* Exploit:
  * A known, confirmed attack
  * Detected when a file or traffic matches a signature pattern (IPS signatures, web application firewall signatures, AV signatures)
  * Examples: Exploit of known application vulnerabilities

How can you protect against Zero-Days?
* Security is not a **fire and forget** endeavor. Study your network's baseline of normal behavior. Monitor unusual
traffic volumes and patterns
* Block traffic that is not needed. Firewall policies must allow only the connections required by applications
* Enforce reasonable protocol constraints: Prevents buffer overflows, cross-site scripting (XSRF), and so on
* Honeypots
* Complement FortiGate with advanced protection (ATP) devices (FortiSandbox, FortiWeb, FortiMail, and so on)
* Train your product security incident response team (PSIRT)
