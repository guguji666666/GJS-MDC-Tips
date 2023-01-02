# Quickly get user token

## 1. Install the Azure Az PowerShell module
https://learn.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-8.3.0
```powershell
$PSVersionTable.PSVersion

Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

Connect-AzAccount -Subscription <subscription id>

$token = Get-AzAccessToken

$token.Token | clip
```
![image](https://user-images.githubusercontent.com/96930989/210188454-74d8a6f2-9941-48b3-88d9-8b16bcc138dd.png)

## 2. Call API with postman
Download postman from https://www.postman.com/downloads/
