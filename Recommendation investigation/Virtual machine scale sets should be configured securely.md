# Reference of remediation of the findings

[Ensure 'Audit Logoff' is set to 'Success'](https://www.tenable.com/audits/items/CIS_DC_SERVER_2016_Level_1_v1.3.0.audit:06f4cbc5b93fc59b2f2dc14ff10a5222)

[Enable 'Scan removable drives' by setting DisableRemovableDriveScanning (REG_DWORD) to 0](https://www.tenable.com/audits/items/CIS_DC_SERVER_2012_Level_1_v2.2.0.audit:df4d1650e8377a7af0759f40c0d848a2)

[Devices: Allow undock without having to log on](https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/devices-allow-undock-without-having-to-log-on)

[Ensure 'Allow Input Personalization' is set to 'Disabled'](https://github.com/ayohrling/local_security_policy/issues/45)

[Ensure 'Allow Cortana' is set to 'Disabled'](https://www.tenable.com/audits/items/CIS_MS_Windows_11_Enterprise_Level_1_Next_Generation_Windows_Security_v1.0.0.audit:d98b72335d81706e4e50e4a55a2a5f77)

[Ensure 'Allow Cortana above lock screen' is set to 'Disabled'](https://www.tenable.com/audits/items/CIS_MS_Windows_10_Enterprise_Level_1_Next_Generation_Windows_Security_v1.7.1.audit:2fb32663ea4cbaac35bc3a3c0694f068)

[Enable 'Send file samples when further analysis is required' for 'Send Safe Samples'](https://www.tenable.com/audits/items/MSCT_Windows_10_1909_1.0.0.audit:0dbab85b152585bab2a2f7a8468e0953)

[Ensure 'Allow Telemetry' is set to 'Enabled: 0 - Security [Enterprise Only]'](https://www.tenable.com/audits/items/CIS_DC_SERVER_2016_Level_1_v1.3.0.audit:ac5ab8e01b5fc023eb2576b03bc05919)

[Ensure 'Allow search and Cortana to use location' is set to 'Disabled'](https://www.tenable.com/audits/items/CIS_MS_Windows_11_Enterprise_Level_1_Next_Generation_Windows_Security_v1.0.0.audit:847d02682604492d0f585e8e2299352c)

[Ensure 'Allow Microsoft accounts to be optional' is set to 'Enabled'](https://www.tenable.com/audits/items/CIS_MS_Windows_11_Enterprise_Level_1_v1.0.0.audit:d90377d3bcf9d74f915f08bc33ea8a6f)

[Ensure 'Network security: Allow Local System to use computer identity for NTLM' is set to 'Enabled'](https://www.tenable.com/audits/items/CIS_MS_Windows_10_Enterprise_Level_1_Next_Generation_Windows_Security_v1.11.0.audit:d57c83fc46374721546e0af549a5bf08)

[Ensure 'Configure Solicited Remote Assistance' is set to 'Disabled'](https://www.tenable.com/audits/items/CIS_MS_Windows_7_v3.2.0_Level_1.audit:56155f2efdb13cc323317c623017af44)

[Ensure 'Block user from showing account details on sign-in' is set to 'Enabled'](https://www.tenable.com/audits/items/CIS_MS_Windows_10_Enterprise_Level_1_v1.9.1.audit:6f202e564eda40c8857449b1fc1ab617)

[Ensure 'Microsoft network server: Digitally sign communications (if client agrees)' is set to 'Enabled'](https://www.tenable.com/audits/items/CIS_MS_Windows_7_v3.2.0_Level_1.audit:b2274f7cb80c7bbc17b8f749e3f8822d)

[Ensure 'Interactive logon: Do not display last user name' is set to 'Enabled'](https://www.tenable.com/audits/items/CIS_MS_Windows_11_Enterprise_Level_1_Next_Generation_Windows_Security_v1.0.0.audit:6ae2b6378849e48ce2b7254c59d3ff22)

[Ensure 'Enable RPC Endpoint Mapper Client Authentication' is set to 'Enabled' (MS only)](https://www.tenable.com/audits/items/CIS_Microsoft_Windows_Server_2016_STIG_v1.1.0_L1_MS.audit:0f08c2bcc89cf26f03d14a0903b9d388)

[Ensure 'Do not show feedback notifications' is set to 'Enabled'](https://www.tenable.com/audits/items/CIS_MS_Windows_10_Enterprise_Level_1_v1.12.0.audit:b8c2e2254c3c8066a8de5e864598d601)

[Ensure 'Do not display the password reveal button' is set to 'Enabled'](https://www.tenable.com/audits/items/CIS_MS_Windows_10_Enterprise_Level_1_v1.9.1.audit:188a107591db9fd598a69e3caf873c71)

[Ensure 'Do not display network selection UI' is set to 'Enabled'](https://www.tenable.com/audits/items/CIS_MS_SERVER_2019_Level_1_v1.2.1.audit:44d163b9b4130054eb1ae5582b6b78b2)

[Ensure 'Prohibit installation and configuration of Network Bridge on your DNS domain network' is set to 'Enabled'](https://www.tenable.com/audits/items/CIS_MS_Windows_10_Enterprise_Level_1_v1.12.0.audit:6a22cb39c4f0b16d7919366c348b008d)

[Ensure 'Windows Firewall: Private: Settings: Display a notification' is set to 'No'](https://www.tenable.com/audits/items/CIS_MS_SERVER_2016_Level_1_v1.3.0.audit:e6d5077fd9035db53dcdf8dee85b1d2b)

[Ensure 'Windows Firewall: Private: Settings: Apply local connection security rules' is set to 'Yes (default)']()

Ensure 'Windows Firewall: Domain: Settings: Display a notification' is set to 'No'
1. Local GPO path: `Computer Configuration -> Windows Settings -> Security Settings -> Windows Firewall with Advanced Security -> Windows Firewall with Advanced Security - Local Group Policy Object` <br>
2. Look for "Windows Firewall: Private: Settings: Apply local connection security rules" and double-click on it. <br>
3. In the properties window that appears, select the "Yes (default)" option. <br>
4. Click on "Apply" and then "OK" to save the changes. <br>


Ensure 'Windows Firewall: Domain: Settings: Apply local connection security rules' is set to 'Yes (default)'
1. Local GPO path: `Computer Configuration -> Windows Settings -> Security Settings -> Windows Firewall with Advanced Security -> Windows Firewall with Advanced Security - Local Group Policy Object` <br>
2. Look for "Windows Firewall: Domain: Settings: Apply local connection security rules" and double-click on it. <br>
3. In the properties window that appears, select the "Yes (default)" option. <br>
4. Click on "Apply" and then "OK" to save the changes. <br>

[Ensure 'Turn off multicast name resolution' is set to 'Enabled'](https://www.tenable.com/audits/items/CIS_DC_SERVER_2016_Level_1_v1.3.0.audit:8c675895143b8d6feeb723329b3935ef)

[Ensure 'Turn off Internet Connection Wizard if URL connection is referring to Microsoft.com' is set to 'Enabled'](https://www.tenable.com/audits/items/CIS_MS_SERVER_2019_Level_2_v1.2.1.audit:a9c09edb661e1860dee91c7d55d21a82)

[Ensure 'Turn off app notifications on the lock screen' is set to 'Enabled'](https://www.tenable.com/audits/items/CIS_MS_Windows_10_Enterprise_Level_1_Bitlocker_v1.12.0.audit:b4116266b4a5f25776e1c3ff50d860aa)

[Ensure 'Setup: Specify the maximum log file size (KB)' is set to 'Enabled: 32,768 or greater'](https://www.tenable.com/audits/items/CIS_Microsoft_Windows_Server_2019_STIG_v1.0.1_L1_MS.audit:0fe31d5780151559690e9003d054d966)

[Ensure 'Prohibit use of Internet Connection Sharing on your DNS domain network' is set to 'Enabled'](https://www.tenable.com/audits/items/CIS_MS_Windows_10_Enterprise_Level_1_v1.7.1.audit:3bef4ee017b99d0ea0d9009972873bcf)

[Ensure 'Windows Firewall: Public: Settings: Apply local connection security rules' is set to 'Yes'](https://www.tenable.com/audits/items/CIS_Microsoft_Windows_Server_2016_STIG_v1.1.0_L1_MS.audit:3bde5902cfa3100253194cddc15d9a78)

[System settings: Use Certificate Rules on Windows Executables for Software Restriction Policies](https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/system-settings-use-certificate-rules-on-windows-executables-for-software-restriction-policies)

[Configure 'Deny access to this computer from the network'](https://www.tenable.com/audits/items/CIS_MS_SERVER_2012_Level_1_v2.1.0.audit:80637caadccc5d048c62d0539ba67471)

[Bypass traverse checking](https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/bypass-traverse-checking)

[Windows Firewall: Public: Allow unicast response]()

[Windows Firewall: Private: Allow unicast response](https://www.tenable.com/audits/items/MSCT_Windows_Server_2012_R2_MS_v1.0.0.audit:5335046c40e3d17d76327daa51d25a39)

Windows Firewall: Domain: Allow unicast response <br>
1. Local GPO path: `Computer Configuration -> Windows Settings -> Security Settings -> Windows Firewall with Advanced Security -> Windows Firewall with Advanced Security - Local Group Policy Object` <br>
2. Locate and double-click on "Windows Firewall: Domain: Allow unicast response." <br>
3. In the properties window that appears, select the "Enabled" option. <br>
4. Click on "Apply" and then "OK" to save the changes. <br>

[Ensure 'Windows Firewall: Public: Settings: Display a notification' is set to 'No'](https://www.tenable.com/audits/items/CIS_MS_Windows_10_Enterprise_Level_1_v1.5.0.audit:fae78338c245a14bc7b7878a33fc827b)

[Ensure 'Deny log on locally' is configured](https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/deny-log-on-locally)

[Ensure 'Deny log on as a batch job' is configured](https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/deny-log-on-as-a-batch-job)

[Ensure 'Deny log on as a service' is configured](https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/deny-log-on-as-a-service)

[Ensure 'Deny log on through Remote Desktop Services' is configured](https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/deny-log-on-through-remote-desktop-services)

[Increase a process working set](https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/increase-a-process-working-set)

[Ensure 'Shut down the system' is set to 'Administrators'](https://www.tenable.com/audits/items/CIS_Microsoft_Windows_Server_2016_STIG_v1.1.0_L1_DC.audit:80529f95c8c29baa924075a74427a665)

[Ensure 'Minimum password length' is set to '14 or more character(s)'](https://www.tenable.com/audits/items/CIS_Microsoft_Windows_Server_2016_STIG_v1.0.0_L3_MS.audit:f35db3ee22b3215c3137a87c04a27150)

[Ensure 'Maximum password age' is set to '70 or fewer days, but not 0'](https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/maximum-password-age)

[Ensure 'Increase scheduling priority' is set to 'Administrators']()

