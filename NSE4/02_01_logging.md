# Loging and Monitoring

### Log types and subtypes
| Traffic logs | Event logs | Security logs |
| --- | --- | --- |
| Forward | Endpoint Control | Application Control |
| Local | High Availability | Antivirus |
| Sniffer | System |  Data Leak Prevention (LDP) |
| | User | Anti-Spam | 
| | Router | Web Filter |
| | VPN | IPS |
| | WAD | Anomaly (DoS-policy) |
| | Wireless | WAF |

### Log Levels
0- Emergency<br />
1- Alert<br />
2- Critical<br />
3- Error<br />
4- Warning<br />
5- Notification<br />
6- Information<br />
7- Debugging: Rarely used<br />

### Log Storage Locations
* Local logging:
  * Memory
  * Hard drive
* Remote logging:
  * Syslog
  * FortiCloud
  * SNMP
  * FortiAnalyzer/FortiManager:
    * They have a list of registered (allowed) devices
    * Fortigate uses port 514 for log transmission
    
### Configureing Log Settings
* Correct time is needed: Manually or through NTP
