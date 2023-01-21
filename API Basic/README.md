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
Connect-AzAccount -TenantId <your tenant id>

Set-AzContext -Subscription <subscription id>

$accessToken = (Get-AzAccessToken).Token

$accessToken | Set-Clipboard
```
![image](https://user-images.githubusercontent.com/96930989/210188454-74d8a6f2-9941-48b3-88d9-8b16bcc138dd.png)

## 3. Retrieve user token and correct the format

The user token has already been copied to clipboard via previous powershell commands

Paste the user token in https://jwt.ms/ and correct the format

Verify that the token could be decoded
![image](https://user-images.githubusercontent.com/96930989/210707002-751e29ab-1380-44f1-afb6-5de4f7e2c75f.png)


