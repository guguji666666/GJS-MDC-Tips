# FIM - File Integrity Monitoring

FIM (file integrity monitoring) uses the Azure Change Tracking solution to track and identify changes in your environment.

## Limitation

[Change Tracking and Inventory overview](https://learn.microsoft.com/en-us/azure/automation/change-tracking/overview?tabs=python-2#current-limitations)

[Current limitation](https://learn.microsoft.com/en-us/azure/automation/change-tracking/overview?tabs=python-2#current-limitations)

<img width="880" alt="image" src="https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/2251b80f-5ddd-4a79-9c77-14041b8d398a">

[Change Tracking and Inventory data collection](https://learn.microsoft.com/en-us/azure/automation/change-tracking/overview?tabs=python-2#change-tracking-and-inventory-data-collection)

<img width="893" alt="image" src="https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/c418c84c-fb5d-4e5c-af24-aceef86e3feb">

Limitation per machine <br>
<img width="886" alt="image" src="https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/a477ddc4-c92c-4f0c-9615-18b21556a288">


## Files and registries recommended to be monitored

[Which files should I monitor?](https://learn.microsoft.com/en-us/azure/defender-for-cloud/file-integrity-monitoring-overview#which-files-should-i-monitor)

| Linux files       | Windows files                                          | Windows registry keys (HKLM = HKEY_LOCAL_MACHINE)            |
| :---------------- | :----------------------------------------------------- | :----------------------------------------------------------- |
| /bin/login        | C:\autoexec.bat                                        | HKLM\SOFTWARE\Microsoft\Cryptography\OID\EncodingType 0\CryptSIPDllRemoveSignedDataMsg{C689AAB8-8E78-11D0-8C47-00C04FC295EE} |
| /bin/passwd       | C:\boot.ini                                            | HKLM\SOFTWARE\Microsoft\Cryptography\OID\EncodingType 0\CryptSIPDllRemoveSignedDataMsg{603BCC1F-4B59-4E08-B724-D2C6297EF351} |
| /etc/*.conf       | C:\config.sys                                          | HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\IniFileMapping\SYSTEM.ini\boot |
| /usr/bin          | C:\Windows\system.ini                                  | HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows    |
| /usr/sbin         | C:\Windows\win.ini                                     | HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon   |
| /bin              | C:\Windows\regedit.exe                                 | HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders |
| /sbin             | C:\Windows\System32\userinit.exe                       | HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders |
| /boot             | C:\Windows\explorer.exe                                | HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run           |
| /usr/local/bin    | C:\Program Files\Microsoft Security Client\msseces.exe | HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce       |
| /usr/local/sbin   |                                                        | HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnceEx     |
| /opt/bin          |                                                        | HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunServices   |
| /opt/sbin         |                                                        | HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunServicesOnce |
| /etc/crontab      |                                                        | HKLM\SOFTWARE\WOW6432Node\Microsoft\Cryptography\OID\EncodingType 0\CryptSIPDllRemoveSignedDataMsg{C689AAB8-8E78-11D0-8C47-00C04FC295EE} |
| /etc/init.d       |                                                        | HKLM\SOFTWARE\WOW6432Node\Microsoft\Cryptography\OID\EncodingType 0\CryptSIPDllRemoveSignedDataMsg{603BCC1F-4B59-4E08-B724-D2C6297EF351} |
| /etc/cron.hourly  |                                                        | HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\IniFileMapping\system.ini\boot |
| /etc/cron.daily   |                                                        | HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\Windows |
| /etc/cron.weekly  |                                                        | HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\Winlogon |
| /etc/cron.monthly |                                                        | HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders |
|                   |                                                        | HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders |
|                   |                                                        | HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Run |
|                   |                                                        | HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\RunOnce |
|                   |                                                        | HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\RunOnceEx |
|                   |                                                        | HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\RunServices |
|                   |                                                        | HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\RunServicesOnce |
|                   |                                                        | HKLM\SYSTEM\CurrentControlSet\Control\hivelist               |
|                   |                                                        | HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\KnownDLLs |
|                   |                                                        | HKLM\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\DomainProfile |
|                   |                                                        | HKLM\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\PublicProfile |
|                   |                                                        | HKLM\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\StandardProfile |


## Services recommend to be monitored
1. **Security and Critical Services:**
   - Windows Defender Antivirus Service (WinDefend)
   - Windows Update (wuauserv)
   - Windows Firewall (MpsSvc)
   - Cryptographic Services (CryptSvc)
   - Remote Procedure Call (RPC, RpcSs)

2. **Networking Services:**
   - DHCP Client (Dhcp)
   - DNS Client (Dnscache)
   - Network Location Awareness (NlaSvc)
   - WLAN AutoConfig (Wlansvc)
   - Wired AutoConfig (dot3svc)

3. **System Health and Monitoring Services:**
   - Windows Management Instrumentation (Winmgmt)
   - Task Scheduler (Schedule)
   - Windows Event Log (EventLog)
   - Performance Logs & Alerts (SysmonLog)
   - Diagnostic Policy Service (DPS, DPS, diagtrack)

4. **User Account and Authentication Services:**
   - Security Accounts Manager (SamSs)
   - Credential Manager (VaultSvc)
   - Smart Card (SCardSvr)
   - Windows Time (W32Time)

5. **Remote Access Services:**
   - Remote Desktop Services (TermService)
   - Remote Access Connection Manager (RasMan)

6. **System Maintenance Services:**
   - Background Intelligent Transfer Service (BITS, BITS, BITS)
   - Windows Modules Installer (TrustedInstaller)
   - Windows Installer (msiserver)

7. **Printing and Spooling Services:**
   - Print Spooler (Spooler)

8. **Domain and Network Services:**
   - Server (LanmanServer)
   - Workstation (LanmanWorkstation)
   - Computer Browser (Browser)
   - Net Logon (Netlogon)
   - TCP/IP NetBIOS Helper (LmHosts)

9. **Windows Update Services (if applicable):**
   - Windows Update (wuauserv)
   - Background Intelligent Transfer Service (BITS, BITS, BITS)

10. **Additional Services (based on your environment):**

   - Application-specific services that are critical for your organization's operations.

Remember that while these are some common services to monitor, the specific services you need to monitor may vary depending on your organization's requirements and the applications you use. Additionally, regularly review and update your list of monitored services as your environment changes.


## KQL query with FIM data

[Support for alerts on configuration state](https://learn.microsoft.com/en-us/azure/automation/change-tracking/overview-monitoring-agent?tabs=win-az-vm#support-for-alerts-on-configuration-state)

| Query                                                        | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| ConfigurationChange \| where ConfigChangeType == "Files" and FileSystemPath contains " c:\windows\system32\drivers\" | Useful for tracking changes to system-critical files.        |
| ConfigurationChange \| where FieldsChanged contains "FileContentChecksum" and FileSystemPath == "c:\windows\system32\drivers\etc\hosts" | Useful for tracking modifications to key configuration files. |
| ConfigurationChange \| where ConfigChangeType == "WindowsServices" and SvcName contains "w3svc" and SvcState == "Stopped" | Useful for tracking changes to system-critical services.     |
| ConfigurationChange \| where ConfigChangeType == "Daemons" and SvcName contains "ssh" and SvcState!= "Running" | Useful for tracking changes to system-critical services.     |
| ConfigurationChange \| where ConfigChangeType == "Software" and ChangeCategory == "Added" | Useful for environments that need locked-down software configurations. |
| ConfigurationData \| where SoftwareName contains "Monitoring Agent" and CurrentVersion!= "8.0.11081.0" | Useful for seeing which machines have outdated or noncompliant software version installed. This query reports the last reported configuration state, but doesn't report changes. |
| ConfigurationChange \| where RegistryKey == @"HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\QualityCompat" | Useful for tracking changes to crucial antivirus keys.       |
| ConfigurationChange \| where RegistryKey contains @"HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\SharedAccess\\Parameters\\FirewallPolicy" | Useful for tracking changes to firewall settings.            |

You can deploy the KQL query in Sentinel analytics rules so that you could get alerts/incidents when relating events are generated.


