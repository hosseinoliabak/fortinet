# SSL VPN
**Comparison between SSL VPN and IPsec VPN:**
* IPsec VPN:
  * IPsec tunnel (ESP layer)
  * Industry standard: Can operates between multiple vendors
  * Login through IPsec client
    * Flexible: Mesh and star topologies
    * For clients or peer gateways
    * Extensible cryptography
  * Installation required
  * Better for office-to-office traffic and data centers  
* SSL VPN
  * HTTPS tunnel (SSL/TLS layer)
  * More bandwidth required because it is in higher layer in OSI model than IP layer
  * Vendor-specific
  * Can be only between FortiClient/Browser and FortiGate
  * Log in through HTTPS web page on FortiGate or use *fortissl* virtual adapter (FortiClient)
  * Simpler setup: no user-configured settings
  * Can be zero-install: OK for Internet cafes and libraries
  * Better for users 

## Modes of SSL VPN
* Web-only: simplest access mode
  * Reverse proxy rewriting of HTTP, HTTPS, FTP, SAMBA (CIFS)
  * Java applets for VNC, Telnet, SSH
  * User's source IP os internal interface IP
* Port Forward (Web-only extension):
  * Requires Java applet
    * Acts as a local proxy that intercepts specific TCP port traffic and encrypts it.
    * Installed without administrator/root privileges
  * Configured through bookmarks on web portal
  * Client applications must point to the java applet  
* Tunnel:
  * Requires FortiClient
  * Install requires administrator/root privilege
  * Accessed through standalone client
  * User traffic source IP address is assigned by FortiGate (VIP), like IPsec
  * Tunnel is only up only while SSL VPN client is connected
  * Split tunneling:
    * Disabled: All traffic including the Internet traffic routed through SSL VPN tunnel to a remote FortiGate then to the destination.
    * Enabled: Only LAN traffic routed through the remote FortiGate; Internet traffic uses the local gateway
    
## Steps to configure SSL VPN
1. Set up accounts and groups
2. Configure portal:
   * Define use access to
     * tunnel
     * web portal
       * By default, same portal for all users: `https://10.0.1.254`
       * Realms: Can make URLs for specialized portals: `https://10.0.1.254/sales`  
     * bookmarks:
       * Not same as your browser's bookmarks
       * Settings for applications that are passing through the VPN tunnel
     * concurrent SSL VPN connections
3. Configure SSL VPN settings
4. Create a firewall policy to accept and decrypt packets
5. Optional: Create a firewall policy to route traffic to the Internet and apply security profiles:
   * When split tunneling is disabled