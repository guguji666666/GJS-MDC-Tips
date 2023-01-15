## Azure VM running Windows TSG steps
#### 1. Check the installation of the qualys extension
#### 2. Test connection to qualys cloud service
[What prerequisites and permissions are required to install the Qualys extension?](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-vm#what-prerequisites-and-permissions-are-required-to-install-the-qualys-extension)

[Locations of qualys cloud service(scroll down)](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-vm#deploy-the-integrated-scanner-to-your-azure-and-hybrid-machines)
* https://qagpublic.qg3.apps.qualys.com (64.39.104.113)- Qualys' US data center
* https://qagpublic.qg2.apps.qualys.eu (154.59.121.74)- Qualys' European data center

If your machine is in a region in an Azure `European geography` (such as Europe, UK, Germany), its artifacts will be processed in Qualys' `European data center`.
Artifacts for virtual machines located `elsewhere` are sent to the `US data center`.


1. Test connection using powershell (launch with admin)
```powershell
Test-NetConnection -Port 443 -ComputerName 64.39.104.113 -InformationLevel Detailed
```
or
```powershell
Test-NetConnection -Port 443 -ComputerName 154.59.121.74 -InformationLevel Detailed
```
![image](https://user-images.githubusercontent.com/96930989/212520515-1f765380-35f5-43d4-a337-349c249549ba.png)


2. Test connection using [Psping](https://learn.microsoft.com/en-us/movere/test-443-connectivity)
```powershell
psping <URL for Resource consumption data upload>:443
```
Sample
```powershell
psping 64.39.104.113:443
```
![image](https://user-images.githubusercontent.com/96930989/212520669-19c546bc-b900-480d-944f-7db5f92d84d2.png)

#### 3. Check if the metadata is generated
Navigate to the path below and see if `Vulnerability_snapshot.db` exists
```
C:\ProgramData\Qualys\QualysAgent\Vulnerability_snapshot.db
```
![image](https://user-images.githubusercontent.com/96930989/212463493-d5981a23-051a-4ca0-b561-6ce8e9cca92e.png)
#### 4. Perform on demand scan, then wait for 24 hours
[Trigger an on-demand scan](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-vm#trigger-an-on-demand-scan)
```cmd
REG ADD HKLM\SOFTWARE\Qualys\QualysAgent\ScanOnDemand\Vulnerability /v "ScanOnDemand" /t REG_DWORD /d "1" /f
```
#### 5. If the issue still persists, reinstall the qualys agent, then wait for 24 hours
Steps to uninstall qualys agent
1. Ensure that the `Qualys Agent` folder is completely removed from the location:
```
C:\ProgramData\Qualys
```
![image](https://user-images.githubusercontent.com/96930989/212525176-0ea6be15-dd56-4806-9d2d-c92085189f82.png)

```
C:\ProgramFiles\Qualys
```
![image](https://user-images.githubusercontent.com/96930989/212525183-c24a0d15-1ca1-4800-b049-302b97a12619.png)

2. Remove the `QualysAgent` from below provided registry path:
```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\
```
![image](https://user-images.githubusercontent.com/96930989/212525215-2331730f-5738-409b-a473-5b383209b91d.png)

3. Remove `Qualys` from below registry location:
```
HKEY_LOCAL_MACHINE\SOFTWARE\
```
![image](https://user-images.githubusercontent.com/96930989/212525234-7335ac4d-86cd-43ac-ae17-c9439067f835.png)

4. Remove the qualys extension from VM
5. Reinstall Qualys agent with the options mentioned in [Automate at-scale deployments](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-vm#automate-at-scale-deployments)

#### 6. Collect the logs below and reach Microsoft support
Log 1
```
C:\WindowsAzure\Logs\Plugins\Qualys.WindowsAgent.AzureSecurityCenter
```
![image](https://user-images.githubusercontent.com/96930989/212525495-f0a7ca13-2140-469d-b2bc-e2a641c98c98.png)

Log 2
```
C:\WindowsAzure\Logs\WaAppAgent.log
```
![image](https://user-images.githubusercontent.com/96930989/212525501-ff596597-a7f6-4b0a-a53e-d6ec865809dc.png)

Log 3
```
C:\Packages\Plugins\Qualys.QualysAgent
```

Log 4
```
C:\Packages\Plugins\Qualys.WindowsAgent.AzureSecurityCenter
```
![image](https://user-images.githubusercontent.com/96930989/212525511-48d64192-fc54-4c26-a781-1240bc2835df.png)

Log 5
```
C:\ProgramData\Qualys
```
![image](https://user-images.githubusercontent.com/96930989/212525523-27dfbd28-6e3c-4fd4-ab20-0414ffcc5c6a.png)


