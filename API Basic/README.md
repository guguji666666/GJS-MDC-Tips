# Quickly get user token and call API

## Get user token using powershell commands
### 1. [Install Azure Az PowerShell module](https://learn.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-8.3.0)
```powershell
$PSVersionTable.PSVersion

Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

Install-Module -Name Az -Scope CurrentUser -Repository PSGallery -Force
```

### 2. Get AAD user token （recommend owner or contributor role on the subscription)
```powershell
Connect-AzAccount -TenantId <your tenant id>

Set-AzContext -Subscription <subscription id>

$accessToken = (Get-AzAccessToken).Token

$accessToken | Set-Clipboard
```
The user access token is already copied to your clipboard.

We can also get user token from Azure cloudshell
```powershell
# Set the subscription
az account set --subscription $subscriptionId

# Get the access token and print it
az account get-access-token --output table --query accessToken
```

### 3. Check user token and correct the format before using it in postman or other tools

The user token has already been copied to clipboard via previous powershell commands

Paste the user token in https://jwt.ms/ and correct the format

Verify that the token could be decoded (below is the sample)
![image](https://user-images.githubusercontent.com/96930989/210707002-751e29ab-1380-44f1-afb6-5de4f7e2c75f.png)

## Use postman
##### 1. Download postman from [Download Postman](https://www.postman.com/downloads/) and launch it.
##### 2. Get user token as mentioned before.
##### 3. Insert the user token here in postman
![image](https://user-images.githubusercontent.com/96930989/210289242-15003c92-1406-4289-9cfd-a08e5cd7260f.png)
##### 4. Set the `request URL`, `Body` if required (below is sample)
![image](https://user-images.githubusercontent.com/96930989/210707768-4979d7d8-4a3e-4b8d-821e-3234f2704be5.png)
