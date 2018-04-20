# Application Control
* Uses IPS engine
* Flow-based (not proxy-based)
* Compares traffic to known application pattern; Only reports packets match for an enabled patterns
* Can detect even if users try to circumvent via an external proxy
* P2P (peer-to-peer) traffic is so difficult to detect; requires more sophisticated UTM

### Application Control Profile
Allows you to filter traffic based on:
* Categories: Similar applications are grouped together; Can view application control signatures for that category
* Application Overrides: Allows you to configure action for specific signatures/applications
* Filter Overrides: Provides a more flexible way to create application categorization based on: behaviour, popularity, risk and so on.

Order of Operations:
* IPS engine identifies the application
* Application control applies:
  1. Application Overrides
  2. Filter Overrides
  3. Categories
### Cloud Access Security Inspection (CASI)
* Allows granular control over popular cloud applications
  * YouTube, Netflix, Dropbox, Hulu, Facebook, and others  
* Applies deeps inspection to traffic-related to cloud services
  * Requires full SSL/SSH inspection in firewall policy