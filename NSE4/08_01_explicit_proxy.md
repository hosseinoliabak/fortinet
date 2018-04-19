# Explicit Proxy
What is a Web Proxy:
* A web proxy forwards client requests to a web server (website). It may cache responses
* Two TCP connections:
  1. From client to web proxy
  2. From web proxy to web server
* Implicit (transparent) proxy: intercepts requests at the IP layer
  * No changes in client's configuration required to implement a web proxy
  * Requests sent to the server's IP address, not the proxy's
* Explicit proxy:
  * Clients must be configured to use an explicit web proxy. Client sends reqquests to the proxy IP and port, not directly to web site
  * The web proxy listens for packets sent to its IP address and port number  
  * By default, is hidden in GUI
  * Policies for explicit proxy are configured in a different configuration section than the regular firewall policies
* How to configure web browsers for explicit proxy?
  * Browser settings: also can be configured through Active Directory logon script or roaming profile
  * Standard Proxy automatic configuration (PAC) file:
    * Usually stored on the web proxy
    * Configure each browser with the PAC file's URL
    * Defines how browsers choose a proxy: FortiGate can act as web server for PAC file
    * `http://<FortiGate_IP>:<port>/proxy.pac`
    * PAC file is a JavaScript file
  * Web proxy auto-discovery protocol (WPAD)
    * WPAD can use two discovery methods:
      * DHCP query: The browser sends a DHCP inform request to the DHCP server. The DHCP server replies with PAC file's URL.
      Browser downloads PAC file
      * DNS query: Very similar to DHCP method. Browser quesries DNS server for the following FQDN: `wpad.<local-domain>`.
      The DNS server replies with the IP address where the PAC file is located. The browser builds the URL using the IP address,
      port 80 and PAC file name *wpad.dat*: `http://<pac-server>:80/wpad.dat`. The browser then downloads the PAC file.
    * Usually, browsers try DHCP method first
      * If DHCP method fails, browsers try DNS method
      * Some browsers only support DNS method

## How to configure explicit web proxy
1. Enable explicit web proxy
2. Indicate which internal interfaces the explicit web proxy will listen on
3. Create explicit proxy policies to allow and inspect traffic
4. Configure each client's browsers to connect through the proxy      