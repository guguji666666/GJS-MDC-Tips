# Install MDE extension manually on Azure VM
* [MDE extension supported Windows OS](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/minimum-requirements?view=o365-worldwide#supported-windows-versions)
* [MDE extension supported Linux OS](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint-linux?view=o365-worldwide#system-requirements)
* [MDE extension supported other OS](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/minimum-requirements?view=o365-worldwide#other-supported-operating-systems)
* [Defender for Servers Plan 2 now integrates with Defender for Endpoint unified solution](https://techcommunity.microsoft.com/t5/microsoft-defender-for-cloud/defender-for-servers-plan-2-now-integrates-with-defender-for/ba-p/3527534)
* [Protect your endpoints with Defender for Cloud's integrated EDR solution: Microsoft Defender for Endpoint](https://learn.microsoft.com/en-us/azure/defender-for-cloud/integration-defender-for-endpoint#new-users-who-never-enabled-the-integration-with-microsoft-defender-for-endpoint-for-windows)


## Deploy MDE extension using Azure policy ( in this way we can define the scope for deployment)
#### 1. Navigate to [Azure Policy](https://portal.azure.com/#view/Microsoft_Azure_Policy/PolicyMenuBlade/~/Definitions)
#### 2. Enter the following in the search bar: "Deploy Microsoft Defender for Endpoint agent on"
#### 3. Choose the policy that matches the device you want to deploy the extension to
![image](https://user-images.githubusercontent.com/96930989/224615665-f277b0d2-36e5-45c0-b892-54d091b949d1.png)
#### 4. Click Assign
#### 5. Select the scope of deployment (Normally Resource Group)
![image](https://user-images.githubusercontent.com/96930989/224615740-2f5a17fa-960d-450f-8088-4b0bdd10280f.png)

#### 6. Click the remediation tab (for the existing VM without MDE extension installed)
#### 7. Check “Create a remediation task”
![image](https://user-images.githubusercontent.com/96930989/224615817-2f29e19b-bbd7-4028-b2bd-3cda04fe72d8.png)

#### 8. Click Review + Create, then Create
#### 9. Then navigate to Remediation > Remediation tasks, you will find the new remediation task created, wait until the task is completed(running backend), this may take several minutes.
![image](https://user-images.githubusercontent.com/96930989/224615924-8116d75a-d93e-4c18-85c4-86c873a8270c.png)

Please notice, the policy is assigned to the resource group:
* All the VM without MDE extension installed will be applied and installed with MDE extension.
* All the VM with MDE extension already installed will remain the same.



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

Request body <br>
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
2. Navigate to Settings > Endpoints  <br>
![image](https://user-images.githubusercontent.com/96930989/224611145-931e10e5-9929-448c-86c0-ec77ab850272.png)

3. Select operating system: Windows 10 and 11
4. Select Deployment method: Microsoft Endpoint Configuration Manager
5. Click "Download onboarding package" <br>
![image](https://user-images.githubusercontent.com/96930989/224611328-e089a895-e0e1-4b5a-bb3b-5a00d99a09a8.png)

6. Unzip/Extract the packaged onboarding script
7. Rename the file "WindowsDefenderATP.onboarding" to "WindowsDefenderATPOnboardingScript.cmd"

Before <br>
![image](https://user-images.githubusercontent.com/96930989/224611653-d85393c3-f1e2-4f9b-84e2-a51e5d1427ee.png)

After <br>
![image](https://user-images.githubusercontent.com/96930989/224611604-2089e85e-d402-4242-a2d0-8dd156aa4634.png)

9. Check powershell version and execute these powerShell commands in the same folder:
Check powershell version
```powershell
$PSVersionTable
```
Sample <br>
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
Sample  <br>
![image](https://user-images.githubusercontent.com/96930989/224612239-e60ae9b5-e851-4618-ba1c-71b3923bd04a.png) <br>
![image](https://user-images.githubusercontent.com/96930989/224612267-d8ebd373-789d-40e6-8509-625c61ad734e.png) <br>
![image](https://user-images.githubusercontent.com/96930989/224612298-3d84c518-7c65-4d80-a27d-7b87b34e4ae1.png) <br>

9. Copy the base64 code and paste it in the “<Base64EncodedPackage>" section  
10. Send the request in the postman
11. Check the MDE extension in Azure VM

  ![image](https://user-images.githubusercontent.com/96930989/224614943-7dcc1b70-84c9-4226-a9ea-4fd971468b23.png) <br>  
  ![image](https://user-images.githubusercontent.com/96930989/224670755-5e5e31eb-dd66-4c22-a39d-aba796f2e494.png) <br>  
  ![image](https://user-images.githubusercontent.com/96930989/224670971-a1991618-c4ab-4ad6-a7e6-928d4a4ea1c2.png)  <br>
  
  
### Install MDE on Azure VM running `Linux` （Updating）
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

3. Select operating system: Linux Server
4. Select Deployment method: Local Script
5. Click “Download onboarding package“
![image](https://user-images.githubusercontent.com/96930989/224672377-386a0165-2bea-4e8e-aaae-f607b865ce46.png)

6. Unzip/Extract the packaged onboarding script
![image](https://user-images.githubusercontent.com/96930989/224673057-7042f509-44ba-4113-8fd9-de081d681ffa.png)

Then create a new python file under the path, let's name it `MDELinux.py`

7. Copy these python commands to `MDELinux.py`, then save it
```python
import base64
f = open('MicrosoftDefenderATPOnboardingLinuxServer.py', 'rb') 
text = f.read() 
f.close() 
base64_encoded_text = base64.b64encode(text)
with open('ouput.txt', 'w') as f:
   f.write(str(base64_encoded_text))
```

8. Run the command
```python
cd <path of python files>
py .\MDELinux.py
```
Sample <br>
![image](https://user-images.githubusercontent.com/96930989/224681648-8ac88c36-bfa2-4e75-8b80-f1fdcc7b7f15.png) <br>

You will then find the `output.txt` under that path, `remove` the leading characters `b'` from the front and the trailing `‘` at the end of the content of output.txt <br>  
![image](https://user-images.githubusercontent.com/96930989/224681935-6577228e-74ef-44b0-9964-ef6dfff87cf9.png) <br>  
![image](https://user-images.githubusercontent.com/96930989/224681985-513d76d7-5645-45b6-8e24-f792fc5fbf8d.png) <br>
  
9. Copy the base64 code and paste it in the `<Base64EncodedPackage>` section  
10. Send the request in the postman
![image](https://user-images.githubusercontent.com/96930989/224683919-d03d871c-2c62-46dd-a1bb-e47b0b8661ae.png)

11. Check the MDE extension in Azure VM
![image](https://user-images.githubusercontent.com/96930989/224683242-4f7c0f47-2a56-4103-83e8-7857c0961f77.png)

  
