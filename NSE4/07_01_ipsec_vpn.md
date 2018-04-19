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

## ISAKMP tunnel
Each endpoint of the tunnel connects and begins to set up the VPN. On first connection, channel is not secure. To exchange
sensitive private keys, both ends have to create a temporary secure channel. How it works:
1. Authenticate peers:
   * Pre-shared key or digital certificate
   * Extended authentication (XAuth)
2. Negotiate one bidirectional SA (called IKE SA)
   * In IKEv1, two possible ways:
     * Main mode: six packets exchanged
     * Aggressive mode: Three packets exchanged
   * Not the same as final SAs later (That is called IPsec SAs)
   * Temporary encrypted tunnel for DH  
3. Diffie-Hellman exchange for secret keys
   * Peers communicate over an unsecure channel; they independently calculate a private key using only public keys
   * Each FortiGate uses shared secret key + nonce (mathematical factor) to calculate key for:
     * Symmetric encryption algorithm such as 3DES, AES
     * Symmetric authentication (HMACs)
     
### NAT Traversal (NAT-T)
ESP can't support NAT as it has no port numbers. If NAT-T is set to *Enable*, it detects if NAT devices exist on the path.
If yes, ESP is encapsulated over UDP 4500. This is recommended when initiator/responder is behind NAT. If NAT-T is set
to *Forces*, ESP is always encapsulated over UDP, even when there are no NAT devices on the path

![image](https://user-images.githubusercontent.com/31813625/38995361-b1cb022e-43b6-11e8-8b4a-d65d91e542c9.png)

## IPsec tunnel
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
