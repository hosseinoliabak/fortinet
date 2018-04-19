# Web Filtering and DNS filtering
Only applies on HTTP traffic not HTTPS
* Flow-based:
  * Analyze TCP flow between client and server
  * Uses IPS engine for inspection
  * Less latency than proxy-based
  * Not as flexible as proxy-based
  * Does not support all website filtering features
  * Intercepts at layer 3s
  * Inspects full URL and offers customizable block page
* Proxy-based
  * Intercepts traffic between client and server
  * Uses a transparent proxy
  * More latency and resource intensive
  * Most inspection options available
  * Intercepts at layer 7
  * Inspects full URL and offers customizable block page
  * DNS-based web filtering available for this mode
    * Based on DNS response
    * FortiGate must use FortiGuard DNS service for DNS lookups; DNS queries redirected to FortiGuard SDNS server 
    * Lightweight: Lacks the precision of HTTP filtering. Low CPU and memory usage
    * Cannot inspect URL, only host name. DNS resolves host name
    * Supports URL filtering and FortiGuard category only

## FortiGuard Category Filter
* Split into multiple categories and sub-categories
* Live connection to FortiGuard: Active contract required. 7-day grace period on expiry
* FortiManager can be used instead of FortiGuard
* how categories are decided?
  * FortiGuard queries the FDN to determine a website category
  * Web filter rating determined by:
    * Human rater
    * Text analysis
    * Exploitation of web structure
* Categories action
  * Allow (Web Filter, DNS filter)
  * Block (Web Filter, DNS filter)
  * Monitor (Web Filter, DNS filter)
  * Warning (Web Filter proxy mode only)
  * Authenticate (Web Filter proxy mode only)
  
## HTTPS scanning
* HTTPS traffic is HTTP traffic encased on a SSL-encrypted tunnel
* Encrypted point-to-point on port 443
* Handled by SSL inspection profile
* SSL Certificate Inspection
  * Reads only encrypted details from hello message 
  * SNI details (if they are present)
  * Certificate CA
* Full SSL Inspection
  * Breaks SSL communication by a proxy
  * Allows full content scanning


HTTP Inspection Order:

![http inspection order](https://user-images.githubusercontent.com/31813625/39015246-c68b6aee-43ea-11e8-8405-1640ed0f72d9.png)

