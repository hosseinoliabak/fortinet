# Loging and Monitoring

* FortiGate date and time should be accurate for effective logging
* NTP server recommended

### Log types and subtypes

There are 3 different types of logs: traffic logs, event logs, security logs

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
0- Emergency: system unstable<br />
1- Alert: immediate action required<br />
2- Critical: functionality affected<br />
3- Error: error exists that can affect functionality<br />
4- Warning: functionality could be affected<br />
5- Notification: information about normal events<br />
6- Information: general system information<br />
7- Debugging: Rarely used; diagnostic information for investigating issues with Fortinet support<br />

### Log Message Layout
* Log header (Similar in all logs)
  * Name of log file: `type`, `subtype` 
  * Severity level: `level`
  * VDOM
* Log body (Varies in all logs)
  * Firewall policy applied to session: `policyid`
  * URL or IP of host: `hostname`
  * Source and destination IP: `srcip`, `dtsip`
  * Action taken by FortiGate: `action`
  * Reason for the action: `msg`

### Log Storage Locations
#### Local logging:
  * Flash Memory for legacy FortiGate devices
  * Hard drive: this type is preferred for local logging
  * Not enabled by default. you must enable disk logging in **Log & Report > Log settings** or:
  <pre>
  # config log disk setting
      set status enable</pre>
  * Not all FortiGate devices support disk logging 
  * By default, logs older than 7 days are deleted from disk
  <pre>
  # config log disk setting
      set maximum-log-age <integer></pre>
  * The system reserves approximately x% of its disk space for system usage and unexpected quota overflow
    * Only ~(1-x)*100% of disk space is available to store logs
    <pre>
    fw-1 (global) # <b>diagnose sys logdisk usage</b> 
    Total HD usage: 241MB/112673MB
    Total HD logging space: 33802MB
    </pre>
#### Remote logging:
  * Syslog
  * FortiCloud: subscription-based service
  * SNMP
  * FortiAnalyzer/FortiManager:
    * They have a list of registered (allowed) devices
    * Fortigate uses UDP/TCP port 514 for log transmission
      * TCP: If you enable logging to FortiAnalyzer using the GUI, reliable logging is auto-enabled
      * UDP: If you enable logging to FortiAnalyzer using the CLI, reliable logging in not auto-enabled. You must manually
      enable using the CLI command.
      <pre>
      # config log fortianalizer setting
          set reliable [enable/disable]
      # config log syslogd setting
          set reliable [enable/disable]    
      </pre>
    * FortiManager
      * Primary purpose: to centralize management of multiple FortiGate devices
      * Like FortiAnalizer, can also store logs and generate reports, but has fixed amount per day that is less than
      equivalent-size FortiAnalyzer (FortiAnalyzer has a log limit of much higher than the equivalent-size FortiManager)
      
    ![image](https://user-images.githubusercontent.com/31813625/38757472-30ce44da-3f3b-11e8-8491-86b744a3ae35.png)

  * Can configure up to 3 separate FortiAnalizer and FortiManager devices for redundancy using CLI.
    <pre>
    fw-1 (global) # <b>config log</b> 
    <b>fortianalyzer</b>     Configure first FortiAnalyzer device.
    <b>fortianalyzer2</b>    Configure second FortiAnalyzer device.
    <b>fortianalyzer3</b>    Configure third FortiAnalyzer device.
    fortiguard        Configure log for FortiGuard.
    memory            Configure memory log.
    syslogd           Configure first syslog device.
    syslogd2          Configure second syslog device.
    syslogd3          Configure third syslog device.
    syslogd4          Configure fourth syslog device.
    webtrends         Configure Web trends.
    </pre>
* FortiGate logs can be sent to syslog servers in CSV or CEF format
    <pre>
    config log syslogd3 setting
        set format [csv | cef]
    end    
    </pre>
* What if FortiAnalyzer Temporarily is unavailable?
  * The FortiGate `miglogd` process catches logs on FortiGate when FortiAnalyzer is not reachable.
  * When maximum catched value is reached, `miglogd` will drop cached logs (oldest first)
  * When FortiAnalyzer connection is back, `miglogd` will send the cached logs.

### Log settings
* Decides if, where, and how a log is stored
  * locally? Enable disk logging
  * historical view (not just real-time)? Enable historical FortiView
  * Remote logging? Upload time? Reliable logging? Encrypted using OFTPS? Configure remote logging
    <pre>
    # config log fortianalizer setting
        set reliable [enable | disable]
        set enc-algorithm [high-medium | high | low | disable]
    </pre>
* Decides whether logs are generated based on your firewall policies
  * log allowed traffic? security events or all sessions? Configure **Log Allowed Traffic** setting on your firewall
  policy (**Policy & Object > IPv4 Policy**)
  * logs from traffic sent through your security profile? Enable one or more security profiles on your firewall policy
  (**Policy & Object > IPv4 Policy**)

#### Hiding user names in logs
* Some laws require that usernames be anonymized
  <pre>
  # config log setting
      set user-anonymize enable
    end    
  </pre>
  * This commands sets the user name in the logs to `anonymous`
