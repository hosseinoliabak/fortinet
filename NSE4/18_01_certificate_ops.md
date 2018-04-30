# Certificate Operations
## PKI
Framework of hardware, software, policies, and procedures that collectively provide a framework for the fundamentals of security
* Authentication
* Confidentiality
* Integrity
* Non-repudiation
* Access control

Core elements:
* Public key cryptography (asymmetric cryptography)
* Certificate authorities (CA): t the root of the hierarchy as a trusted 3rd party in model of trust relationships
  * CA issues its own digital certificate AKA root certificate in order to establish this point of ultimate trust

### Cryptography
Protects the data exchanged between two parties in an electronic transaction. Includes the following elements:
* Data privacy (Confidentiality): Achieved by encryption. Data is private while in transit. Only intended recipient with
right key can decipher the data
* Data integrity: Achieved by hashes. Ensures that the data is not altered in transit (CHKSUM of data on recipient
matches CHKSUM of data on sender). Sender sends the cipher and the hash. Then, the recipient recovers the plaintext and 
recalculates the hash
* Authentication: Recipient can verify the identity of trusted senders; allows confirmation of the data's origin
* Non-repudiation: Sender cannot deny participating in the transaction at a later date

### PKI Standards 
* X.509: AKA ITU-T standard
* PKCS: invented by RSA corporation; de facto standard for a lot of PKI systems 
  * PKCS#1: RSA Cryptography Standard 
  * PKCS#3: Diffie-Hellman key exchange
  * PKCS#10: This is a format of a certificate request sent to a CA that wants to receive its identity certificate
  This type of request would include the public key for the entity desiring a certificate
  * PKCS#7: This is a format that can be used by a CA as a response to a PKCS#10 request. The response itself will very
  likely be the identity certificate (or certificates) that had been previously requested
  * PKCS#12: A format for storing both public and private keys using a symmetric password-based key to *unlock* the data
  whenever the key needs to be used or accessed

### Digital Certificate (X.509 Certificate)
* Contains the public key
* A digital credential that identifies an end entity (user or network service)

### Types of Digital Certificates
* **CA Certificates:** AKA root or authority certificates 
  * Identifies the CA
  * Creates root of hierarchy
  * Issuer and subject field the same in certificate details
  * Self-signed
  * Contain CA's public key
  
* **Web Server Certificates:** AKA local service certificates
  * Identifies the web server
  * Used to secure communication to and from web servers (ex. SSH, HTTPS, web portals, or EAP 802.1X authentication servers)
  * Subject field usually contains DNS name or IP address of the server
  * When a browser requests an HTTPS website, it receives the Website's server certificate, which is signed by the root
  CA that issued it
  * Browser must trust the CA to authenticate the certificate
    * To trust the CA, the CA certificate must exist in the browser's list of trusted CAs

* **User (client) Certificates:**
  * Identifies one person to another, a person to a device or gateway, or one device to another
  * Contains public key associated with the identity
  * User certificate includes signature of CA (encrypted using the private key of the CA) and user's public key
  * To authenticate with a user certificate, the authentication server (ex. FortiGate) must have the CA certificate that signed
the user certificate.
    * The CA certificate contains the CA's public key, allowing the authentication server to decrypt/validate anything
  encrypted/signed by the CA's private key 
  
### Certificate Revocation Lists (CRLs):
* CA can revoke a certificate (for example, if private key is compromised) or an employee leaves the organization
* Revoked certificates listed on CRL for the CA that signed it
* CRL remotely accessible
* CRL updated and re-posted periodically
* When entities attempt to validate the certificate, they can see it's on the CRL and decide not to trust it

## Managing digital certificates
### Generating a Certificate
1. Generate a Certificate Signing Request (CSR) to a CA server for FortiGate (in GUI: **System > Certificates**).
A private and public key pair is created for your FortiGate. the CSR (FortiGate, PuK) is signed by the FortiGate Private key
2. Submit the CSR to a CA - PKCS#10. CSR includes the FortiGate Public key and information about the FortiGate itself.
Note that the private key remains confidential on FortiGate
3. CA - out of band - verifies if the information in the CSR is valid and then creates a digital certificate for FortiGate.
The certificate is digitally signed using the CA's private key. The CA also stores the certificate in a central repository
4. CA sends back the Identity Certificate and public key of the FortiGate. CA publishes the public key bound to FortiGate (FortiGate, PuK)
5. Install the Certificate on your FortiGate

### Certificate-based authentication for Administrators, SSL VPN clients, and IPsec VPN peers

#### Creating PKI peer user accounts
The root CA certificate(s) and the CRL(s) must be installed on FortiGate!
1. Create a PKI peer user account (CLI for the first user)
2. Create a firewall user group for PKI peer users
3. Assign PKI peer user to user group

Note the user must install their certificate in the personal certificate store in their computer.
* Firefox: uses its own certificate repository
* IE and chrome: use the certificate repository of OS

#### Creating PKI Administrator Accounts

The root CA certificate(s) and the CRL(s) must be installed on FortiGate!
1. Create a PKI administrator
   * Create PKI peer user account and administrator account (2 accounts)
2. Add the PKI peer user account to a firewall user group
3. Configure the administrator acocunt
   * Select **Use public key infrastructure (PKI) group** as the account type and select the PKI group to which the PKI
   peer user belong

#### Certificate authentication: SSL VPN and IPsec VPN

* FortiGate can use certificates to authenticate both SSL VPN clients and IPsec VPN peers: Requiures root CA certificate on FortiGate
* Configuration more complex: `docs.fortinet.com` *FortiOS Administration Guide*
* Anytime you use certificate authentication (users, administrators, or VPN), always make sure you import the CRL. Keep the CRL updated.