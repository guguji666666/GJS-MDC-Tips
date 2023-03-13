# Install MDE extension manually on Azure VM
* [MDE extension supported Windows OS](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/minimum-requirements?view=o365-worldwide#supported-windows-versions)
* [MDE extension supported Linux OS](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint-linux?view=o365-worldwide#system-requirements)
* [MDE extension supported other OS](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/minimum-requirements?view=o365-worldwide#other-supported-operating-systems)

## Deploy MDE extension using Azure policy
![image](https://user-images.githubusercontent.com/96930989/224591445-26a54866-e2aa-429a-a906-6ef37687b15c.png)


## Deploy MDE extension using API
### Install MDE on Azure VM running `Windows`
#### 1. [Download and install postman](https://www.postman.com/downloads/)
#### 2. [Get user token](https://github.com/guguji666666/GJS-MDC-Tips/tree/main/API%20Basic)
#### 3. Insert the user token here in postman
![image](https://user-images.githubusercontent.com/96930989/210289242-15003c92-1406-4289-9cfd-a08e5cd7260f.png)


Request body
```json
{ 
"name": "MDE.<OsType>", 
"id": "<ResourceId>/extensions/MDE.<OsType>", 
"type": "Microsoft.<MachineType>/<MachineTypePath>/extensions", 
"location": "<location_of_vm>", 
"properties": { 
"autoUpgradeMinorVersion": true, 
"publisher": "Microsoft.Azure.AzureDefenderForServers", 
"type": "MDE.<OsType>", 
"typeHandlerVersion": "1.0", 
"settings": { 
"azureResourceId": "<ResourceId>", 
"defenderForServersWorkspaceId": "<SubscriptionId>", 
"forceReOnboarding": true 
}, 
"protectedSettings": { 
"defenderForEndpointOnboardingScript": "<Base64EncodedPackage>" 
} 
} 
} 
```

* OsType: Windows / Linux

* ResourceId: Azure Resource Id of the Azure VM

* MachineType: Compute (for Azure VM) / HybridCompute (for Azure Arc)

* MachineTypePath: virtualMachines (for Azure VM) / machines (for Azure Arc)

* ApiVersion: 2015-06-15 (for Azure VM) / 2020-08-02 (for Azure Arc)

#### Get `<Base64EncodedPackage>` for Windows VM
1. Select operating system: Windows 10 and 11

2. Select Deployment method: System Center Configuration Manager 2012 / 2012 R2 / 1511 / 1602

3. Click “Download onboarding package“

4. Unzip/Extract the packaged onboarding script

5. Rename the file "WindowsDefenderATP.onboarding" to "WindowsDefenderATPOnboardingScript.cmd"

6. Check powershell version and execute these PowerShell commands in the same folder:
Check powershell version
```powershell
$PSVersionTable
```
Sample

![image](https://user-images.githubusercontent.com/96930989/224610535-ad3ccc11-f5f8-4048-a2f8-0599d4191f83.png)

For Windows PowerShell versions > 7.0 (included) first line of the script needs to be changed as below:
```powershell
cd <path of the onboarding package>
$byteContent = Get-Content -Path "WindowsDefenderATPLocalOnboardingScript.cmd" -AsByteStream
$base64_encoded_text = [System.Convert]::ToBase64String($byteContent)
$base64_encoded_text >> output.txt
```

For Windows PowerShell versions < 7.0
```powershell
cd <path of the onboarding package>
$byteContent = Get-Content -Path "WindowsDefenderATPOnboardingScript.cmd" -Encoding Byte
$base64_encoded_text = [System.Convert]::ToBase64String($byteContent)
$base64_encoded_text >> output.txt
```
7. Copy the base64 code and paste it in the “<Base64EncodedPackage>" section
  
8. Send the request in the postman
  
### Install MDE on Azure VM running `Linux`
#### 1. [Download and install postman](https://www.postman.com/downloads/)
#### 2. [Get user token](https://github.com/guguji666666/GJS-MDC-Tips/tree/main/API%20Basic)
#### 3. Insert the user token here in postman
![image](https://user-images.githubusercontent.com/96930989/210289242-15003c92-1406-4289-9cfd-a08e5cd7260f.png)
