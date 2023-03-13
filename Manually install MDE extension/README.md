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

Request URL
```
https://management.azure.com/ <ResourceId>/extensions/MDE.<OsType>?api-version=<ApiVersion>
```

Binding
```
PUT
```

Request body
![image](https://user-images.githubusercontent.com/96930989/224613494-7382c921-c898-4010-8e70-d1bdb156ec2b.png)

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
1. Navigate to [MDE portal](https://security.microsoft.com)
2. Navigate to Settings > Endpoints
![image](https://user-images.githubusercontent.com/96930989/224611145-931e10e5-9929-448c-86c0-ec77ab850272.png)

3. Select operating system: Windows 10 and 11
4. Select Deployment method: Microsoft Endpoint Configuration Manager
5. Click “Download onboarding package“
![image](https://user-images.githubusercontent.com/96930989/224611328-e089a895-e0e1-4b5a-bb3b-5a00d99a09a8.png)

6. Unzip/Extract the packaged onboarding script
7. Rename the file "WindowsDefenderATP.onboarding" to "WindowsDefenderATPOnboardingScript.cmd"

Before

![image](https://user-images.githubusercontent.com/96930989/224611653-d85393c3-f1e2-4f9b-84e2-a51e5d1427ee.png)

After

![image](https://user-images.githubusercontent.com/96930989/224611604-2089e85e-d402-4242-a2d0-8dd156aa4634.png)

9. Check powershell version and execute these powerShell commands in the same folder:
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
Sample:
![image](https://user-images.githubusercontent.com/96930989/224612239-e60ae9b5-e851-4618-ba1c-71b3923bd04a.png)

![image](https://user-images.githubusercontent.com/96930989/224612267-d8ebd373-789d-40e6-8509-625c61ad734e.png)

![image](https://user-images.githubusercontent.com/96930989/224612298-3d84c518-7c65-4d80-a27d-7b87b34e4ae1.png)

9. Copy the base64 code and paste it in the “<Base64EncodedPackage>" section  
10. Send the request in the postman
11. Check the MDE extension in Azure VM

  
  
  
  
### Install MDE on Azure VM running `Linux`
#### 1. [Download and install postman](https://www.postman.com/downloads/)
#### 2. [Get user token](https://github.com/guguji666666/GJS-MDC-Tips/tree/main/API%20Basic)
#### 3. Insert the user token here in postman
![image](https://user-images.githubusercontent.com/96930989/210289242-15003c92-1406-4289-9cfd-a08e5cd7260f.png)
