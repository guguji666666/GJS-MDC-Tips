# D4SQL AMA auto-provisioning

## Before we start
### List defender for cloud price tier [Pricings - List - REST API (Azure Defender for Cloud) | Microsoft Learn](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/pricings/list?view=rest-defenderforcloud-2024-01-01&tabs=HTTP)
#### Sample output (i have enabled AMA auto-provisioning in D4SQL)
Binding
```
GET
```
URL
```
https://management.azure.com/{scopeId}/providers/Microsoft.Security/pricings?api-version=2024-01-01
```
Get user token to be used in postmam
```powershell
Set-ExecutionPolicy RemoteSigned

Connect-AzAccount -TenantId <tenant id>

Set-AzContext -Subscription <subscription id>

$accessToken = (Get-AzAccessToken).Token

$accessToken | Set-Clipboard
```


## 1. Enable `Azure Monitoring Agent for SQL server on machines` in panel
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/ea4a6d04-ceb2-4b94-bf9d-6f0bd0060539)

### Activity logs
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/ec092b04-7f13-47f2-ab5f-cd45827a1aa8)

### Remediation tasks automatically created
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/8293c4d4-3a22-4b8e-8fce-c88477882a74)

#### Conclusion
When enabling `Azure Monitoring Agent for SQL server on machines` in panel, relating polices are assgined automatically and so do remediation tasks


## 2. Assign initiative "Configure SQL VMs and Arc-enabled SQL Servers to install Microsoft Defender for SQL and AMA with a user-defined LA workspace"

### Initiative info
```
"/providers/Microsoft.Authorization/policySetDefinitions/de01d381-bae9-4670-8870-786f89f49e26"
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/97c458a9-2a84-47f4-9075-a4c0ba43b299)


#### Assign the intiative to subscription level manually
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/6d88bacf-d0a5-4716-9bac-57c2faa1a726)

#### `Azure Monitoring Agent for SQL server on machines` is then be enabled and configured
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/ea9e6d9c-e35c-48cc-96f8-f46fc3789c39)


#### Conclusion
When assigning initiative "Configure SQL VMs and Arc-enabled SQL Servers to install Microsoft Defender for SQL and AMA with a user-defined LA workspace" to the subscription level, although the poclices inside doesn't reach directly to price tier or extension below it, the function `Azure Monitoring Agent for SQL server on machines` will still be enabled, which looks like a backend behavior
