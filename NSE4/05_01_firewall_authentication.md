# Firewall Authentication
Includes authentication of users and user groups
## Active Authentication
Active authentication means that user receives a login prompt. He/She must manually enter credentials to authenticate.
* Local password authentication: username and password stored on FortiGate
* Server-based password authentication (AKA remote password authentication): Password stored on a POP3, RADIUS, LDAP, and TACACS+ server
  * FortiGate sends the user entered credentials to the remote server for verification
  * Only LDAP can have a secure connection with FortiGate using a server certificate
* Two-factor authentication: enabled on top of existing method; requires something you know and something you have (token or certificate)
  * Two-factor authentication ond One-Time Password (OTP):
    * OTPs one-time use only
    * OTPs can be time-based or event-based
    * FortiToken 200 / FortiToken Mobile: Generate a 6-digit code every 60 seconds based on a unique seed and GMT time
    * Email / SMS: One-time password sent to user's email or SMS respectively
    * NTP server recommended
    * User & Device > FortiTokens
  * Two-factor authentication and Certificates
    * Can add a PKI user account on FortiGate
      * Must add first user through CLI
        ```
        # config user peer
            edit <username>
                set cn <cn of user cert>
                set ca <CA that issued user cert>
                set two-factor enable
                set passwd <password>
        ```
      * Subsequent users through GUI: User & Device > PKI
    * Can include PKI users in firewall user groups (or peer certificate groups used in IPsec VPNs)

Firewall policy must allow the HTTP, HTTPS, FTP, and/or Telnet protocols
## Passive Authentication    
Passive authentication means that the user does not receive a login prompt. Credentials are determined automatically.
Methods varies depending on type of authentication used:
* FSSO, RSSO, and NTLM

## User Groups
Type of user groups:
* Firewall: Provides access to firewall policies that required authentication
* Guest: Mostly commonly used for guest access in wireless networks; contain temporary accounts
* FortiGate Single Sign On (FSSO)
* RADIUS single sign on (RSSO)