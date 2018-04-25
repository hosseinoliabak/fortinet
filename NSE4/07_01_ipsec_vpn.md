# IPsec VPN
IPsec is an RFC (vendor-neutral) standard. IPsec provides:
* Authentication
* Data integrity (HMAC): tamper proofing
* Data confidentiality: encryption

Really, IPsec is a suite of separate protocols:
* IKE - RFC 2409 (IKEv1), RFC 4306 (IKEv2):
  * NAT: IP protocol 17, UDP 500 or 4500 (for rekey, quick mode, mode-cfg)
  * NO NAT: IP protocol 17, UDP 500
* ESP - RFC 4303:
  * NAT: IP protocol 17, UDP 4500
  * No NAT: IP protocol 50
* AH: IP protocol 51; provides integrity but no encryption. It is not used by FortiGate. (ESP is used by FortiGate)  

Encapsulation: Tunnel or transport mode

![ipsec encapsulation](https://user-images.githubusercontent.com/31813625/38993652-3a2e5512-43b2-11e8-878c-3f363aaf2f6c.png)

Negotiation: Security Association (SA):
Bundle of separate algorithms and parameters being used to encrypt and authenticate data travelling through the tunnel.
In a two-way traffic, this exchange is secured by a pair of SAs, one for each traffic direction. Both sides of the tunnel must 
agree on the security rules. IKE uses two distinct phases:
* Phase 1: ISAKMP tunnel
* Phase 2: IPsec tunnel

## ISAKMP tunnel (Phase 1)
Each endpoint of the tunnel connects and begins to set up the VPN. On first connection, channel is not secure. To exchange
sensitive private keys, both ends have to create a temporary secure channel. How it works:
1. Authenticate peers:
   * Pre-shared key or digital certificate
   * Extended authentication (XAuth)
2. Negotiate one bidirectional SA (called IKE SA)
   * In IKEv1, two possible ways:
     * Main mode: six packets exchanged
       * Initiator sends suggested ISAKMP policies
       * Responder selects which policy to use and replies with selected ISAKMP policies
       * Initiator sends its key
       * Responder replies with its own key
       * Initiator sends its peer ID and hash payloads
       * Responder replies its peer ID and hash payloads
       * In this case, the responder can identify the initiator only by the source IP address of the first packet. Nothing else.
       This is because the first packet does not contain the peer ID. That works well for poin-to-point VPNs because the
       responder knows the IP address of each peer. 
     * Aggressive mode: Three packets exchanged
       * Initiator sends suggested ISAKMP policy, its key, and peer ID
       * The responder responds with the same information of its own plus hash payload
       * Finally, initiator sends its hash payload
       * Unlike the main mode, the first packet contains the initiator's peer ID. Therefore, the responder can use this ID
       to identify who the peer is and which security policy to use.
   * Phase 1 authentication is usually weak pre-shared keys. XAuth adds more, especially for mobile users: user name + password
     * Sometimes called phase 1.5
   * Not the same as final SAs later (That is called IPsec SAs)
   * Temporary encrypted tunnel for DH  
3. Diffie-Hellman exchange for secret keys
   * Peers communicate over an unsecure channel; they independently calculate a private key using only public keys
   * Each FortiGate uses shared secret key + nonce (mathematical factor) to calculate key for:
     * Symmetric encryption algorithm such as 3DES, AES
     * Symmetric authentication (HMACs)

### Type of remote peers
* Static IP address: The peer has a static IP: predictable
  * Can be either initiator or responder
* Dynamic DNS: The peer has a dynamic IP, but its DNS domain is static: predictable
  * Can be either initiator or responder
* Dialup: The peer has dynamic IP and no dynamic DNS: unpredictable
  * Can be a responder only (not FortiGate as an initiator). It won't know where to send VPN connection requests
  * On the Hub:
    * Phase 1:
      * NAT T usually should be enabled because our mobile users are usually behind NAT
      * Remote Gateway must be set to Dialup user
      * Peer Options with aggressive mode must be used for multiple dialup VPNs
      * Probably you configure **Mode Config** because it is not practical to assign static IP to mobile users. Like DHCP,
    automatically configures VPN clients' virtual network settings
    * Phase 2 Selector Configuration:
      * Local address: Hub's subnet
      * Remote address: 0.0.0.0/0 for matching multiple spokes subnets
  * On the Spokes:
    * Phase 1:
      * Remote Gateway must be either static IP or Dynamic DNS
      * Local ID and aggressive moe required for multiple dialup VPNs  
    * Phase 2 Selector Configuration:
      * Local address: Spoke's subnet - static route to this subnet will be automatically added on the hub
      * Remote address: Hub's subnet
      
### NAT Traversal (NAT-T)
ESP can't support NAT as it has no port numbers. If NAT-T is set to *Enable*, it detects if NAT devices exist on the path.
If yes, ESP is encapsulated over UDP 4500. This is recommended when initiator/responder is behind NAT. If NAT-T is set
to *Forces*, ESP is always encapsulated over UDP, even when there are no NAT devices on the path

![image](https://user-images.githubusercontent.com/31813625/38995361-b1cb022e-43b6-11e8-8b4a-d65d91e542c9.png)

## IPsec tunnel (Phase 2)
Once phase 1 has established a somewhat secure channel and private keys, phase 2 begins.
1. Negotiates two unidirectional SAs for ESP (Called IPsec SAs); protected by phase IKE SA
2. When SAs are about to expire, renegotiates. This maintains security. Optionally, if you enable Perfect Forwarding Secrecy (PFS),
each time phase 2 expires, FortiGate will use DH to recalculate new secret keys.

Each phase 1 can have multiple phase 2s. When would this happen? For example, you may want to use different encryption key
for each subnet whose traffic is crossing the tunnel (High security subnets can have stronger ESP.) How does FortiGate select whihc phase 2 to use? By checking which
quick mode selector the traffic matches.

IKE phase 2 has only one mode which is called **quick mode**
* During phase 2, we must configure a pair of settings called *quick mode selectors* (IPsec Transform Set).
They identify and direct the traffic to the appropriate phase 2. Selectors behave similarly to a firewall policy.
VPN traffic must match selectors in one of the phase 2 SAs. If it does not, the traffic is dropped.

## Route-based compared to Policy-based Configuration
* Generally, route-based VPNs offer more control and flexibility.

| Feature | Route-based (Interface-based) | Policy-based |
| --- | --- | --- |
| Operation mode | Only NAT | NAT and transparent |
| L2TP-over-IPsec | Yes | Yes
| GRE-over-IPsec | Yes | No
| Routing Protocols | Yes | No
| Number of policies per VPN | 2 policies with action **ACCEPT** usually one for each direction | One policy with action **IPSEC** controls both traffic directions |
| Hidden in GUI by default | No | Yes | 

* You can use policy-based on one side and route-based on the other side

## VPN Topologies
* Point-to-point (site-to-site)
* Point-to-multipoint (Dialup): Only clients know destination IP and VPN settings
* Built by combining point-to-point and dialup VPNs
  * Hub-and-spoke
  * Full meshed
  * Partial meshed
  
## Auto Discovery VPN (ADVPN)
* Like Cisco's DMVPN
* Dynamically negotiate on-demand direct VPNs between spokes
* Provides the benefit of a full-meshed topology over a hub-spoke/partial-mesh deployment
* Requires the use of routing protocol for spokes to learn the routes to other spokes 
* The administrator enable the following IPsec phase 1 settings:
  * `auto-discovery-receiver` in the spokes' VPN
  * `auto-discovery-sender` and `auto-discovery-psk` in the hubs' VPN
  * `auto-discovery-forwarded` in the hubs' VPN that go to each other hub
  
## Redundant VPN

Partially redundant: One peer has two connections:
  ![vpn partial redundant](https://user-images.githubusercontent.com/31813625/39252643-14b0daa0-4874-11e8-93a4-0a8cef04d86c.png)

Fully redundant: Both peers have two connections:
  ![vpn fully redundant](https://user-images.githubusercontent.com/31813625/39252813-75a98e24-4874-11e8-91c7-0638b4fc7f06.png)

Configuration:
1. Add one route-based phase 1 configuration for each tunnel. Dead Peer Detection (DPD) must be enabled on both ends
2. Add at least one phase 2 definition for each phase 1
3. Add one static route for each path. Use distance or priority to select primary over backup routes. Alternatively,
use dynamic routing
  ![vpn redundant](https://user-images.githubusercontent.com/31813625/39253623-3fddd104-4876-11e8-9282-649e950adb88.png)
4. Configure firewall policies for each interface to allow the traffic through both the primary and backup VPNs
