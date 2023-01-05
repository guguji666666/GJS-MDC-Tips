# Quickly get user token and call API

## 1. Install the Azure Az PowerShell module
https://learn.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-8.3.0
```powershell
$PSVersionTable.PSVersion

Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

Install-Module -Name Az -Scope CurrentUser -Repository PSGallery -Force
```

## 2. Get AAD user token （recommend owner or contributor role on the subscription)

```powershell
Connect-AzAccount -Subscription <subscription id>

$token = Get-AzAccessToken

$token.Token | clip
```
![image](https://user-images.githubusercontent.com/96930989/210188454-74d8a6f2-9941-48b3-88d9-8b16bcc138dd.png)

## 3. Retrieve user token and correct the format

The user token has already been copied to clipboard via previous powershell commands

Paste the user token in https://jwt.ms/ and correct the format

Remove the part highlighted
![image](https://user-images.githubusercontent.com/96930989/210706776-556e1185-6744-4c1a-9fda-6d9044cb2f34.png)

Verify that the token could be decoded then
![image](https://user-images.githubusercontent.com/96930989/210707002-751e29ab-1380-44f1-afb6-5de4f7e2c75f.png)


## 2. Call API with postman
Download postman from https://www.postman.com/downloads/

# Manually install ASA
https://learn.microsoft.com/en-us/azure/defender-for-cloud/auto-deploy-azure-monitoring-agent#additional-extensions-for-defender-for-cloud
```
The Azure Monitor Agent requires additional extensions. 
The ASA extension, which supports endpoint protection recommendations, fileless attack detection, and Adaptive Application controls, is automatically installed when you auto-provision the Azure Monitor Agent.
```

## Install ASA on Azure VM running Windows
#### 1. Get the user token as mentioned above
#### 2. Launch postman

`Binding`: PUT

`URL`
```
https://management.azure.com/subscriptions/<subdid>/resourceGroups/<rgname>/providers/Microsoft.Compute/virtualMachines/<vmname>/extensions/AzureSecurityWindowsAgent?api-version=2019-03-01
```

`Body`
```
{
  "name":"AzureSecurityWindowsAgent", 
  "type":"Microsoft.Compute/virtualmachines/extensions", 
  "location":"<vmlocation>", 
  "properties":{ 
    "autoUpgradeMinorVersion":true, 
    "publisher":"Microsoft.Azure.Security.Monitoring", 
    "type":"AzureSecurityWindowsAgent", 
    "typeHandlerVersion":"1.0",
    "settings":{ },
    "protectedsettings": {}
}
}
```
#### 4. Insert the user token here
![image](https://user-images.githubusercontent.com/96930989/210289242-15003c92-1406-4289-9cfd-a08e5cd7260f.png)

#### 5. Send the request
