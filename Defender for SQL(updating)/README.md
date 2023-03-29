# Microsoft Defender for Azure SQL

* [Overview of Microsoft Defender for Azure SQL](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-sql-introduction)

* [SQL VA results reference list](https://learn.microsoft.com/en-us/azure/defender-for-cloud/sql-azure-vulnerability-assessment-rules#data-protection)

* [Find and remediate vulnerabilities in your Azure SQL databases](https://learn.microsoft.com/en-us/azure/defender-for-cloud/sql-azure-vulnerability-assessment-find?tabs=express)

* [Manage vulnerability findings in your Azure SQL databases](https://learn.microsoft.com/en-us/azure/defender-for-cloud/sql-azure-vulnerability-assessment-manage?source=recommendations&tabs=express#faq)

* [New express configuration for Vulnerability Assessment in Microsoft Defender for SQL- Public Preview](https://techcommunity.microsoft.com/t5/microsoft-defender-for-cloud/new-express-configuration-for-vulnerability-assessment-in/ba-p/3695390)

![image](https://user-images.githubusercontent.com/96930989/224294846-8dc69bff-91d9-44aa-b7b5-fa96a0a404cc.png)

![image](https://user-images.githubusercontent.com/96930989/224294881-80429837-8385-40c4-8437-2b6395c432bc.png)

## Useful powershell commands
### 1. Define storage account to store defender for SQL VA results
* Apply to configuration at the server side (defender for SQL should be disabled at subscription level)
* Support storage account with puiblic access or behind firewall
* The storage account and the SQL server should be in the same resource group (we can modify the storage account later)

```powershell
$ResourceGroupName = '<name of the resource group that SQl server belongs to>'
$ServerName = '<name of the SQL server to be enabled defender plan>'
$DatabaseName = '<name of the sql database>'
$SubscriptionId = "<id of the subscription>"
$StorageAccountName = "<name of the storage account you want to use"

# Connect Azure 
Connect-AzAccount
Set-AzContext -SubscriptionId $SubscriptionId

# Generate/Update a Managed Identity for the SQL Server
Set-AzSqlServer -AssignIdentity -ResourceGroupName $ResourceGroupName -ServerName $ServerName

# Get the SQL Server information, as we need the identity that was generated.
$sqlServer = Get-AzSqlServer -ResourceGroupName $ResourceGroupName -ServerName $ServerName

# Assign the Managed Identity a "Storage Blob Data Contributor" role on the Storage account.
New-AzRoleAssignment `
    -ObjectId $sqlServer.Identity.PrincipalId.Guid `
    -RoleDefinitionName 'Storage Blob Data Contributor' `
    -ResourceName $StorageAccountName `
    -ResourceType 'Microsoft.Storage/storageAccounts' `
    -ResourceGroupName $ResourceGroupName

# Enable Advanced Data Security
Enable-AzSqlServerAdvancedDataSecurity -DoNotConfigureVulnerabilityAssessment -ResourceGroupName $ResourceGroupName -ServerName $ServerName

# Set VA policy with an empty SAS Key, this indicates we are using Managed Identity
Update-AzSqlServerVulnerabilityAssessmentSetting `
    -ResourceGroupName $ResourceGroupName `
    -ServerName $ServerName `
    -BlobStorageSasUri "https://$StorageAccountName.blob.core.windows.net/vulnerability-assessment?" `
    -RecurringScansInterval Weekly
```

#### Sample in my lab

Defender for SQL is not enabled yet

![image](https://user-images.githubusercontent.com/96930989/228437517-4cbbdd91-5328-4e33-b466-a3d0db7a0a7e.png)


```powershell
$ResourceGroupName = 'GJS-MS150-MDFC2'
$ServerName = 'gjs-test-sql2'
$DatabaseName = 'gjs-ms-sqdb3'
$SubscriptionId = "74a72629-ac6d-44db-a66a-abc69f3bfb7e"
$StorageAccountName = "gjs666storage1"
```

##### Connect Azure 
```powershell
Connect-AzAccount
Set-AzContext -SubscriptionId $SubscriptionId
```
![image](https://user-images.githubusercontent.com/96930989/228437788-e2571c68-9577-4361-b5ca-2b0d4b71312c.png)

##### Generate/Update a Managed Identity for the SQL Server
```powershell
Set-AzSqlServer -AssignIdentity -ResourceGroupName $ResourceGroupName -ServerName $ServerName
```
![image](https://user-images.githubusercontent.com/96930989/228437991-01b4b755-c7c5-4bbf-8ee9-c84f19d6b6c8.png)


##### Get the SQL Server information, as we need the identity that was generated.
```powershell
$sqlServer = Get-AzSqlServer -ResourceGroupName $ResourceGroupName -ServerName $ServerName
```

##### Assign the Managed Identity a "Storage Blob Data Contributor" role on the Storage account.
```powershell
New-AzRoleAssignment `
    -ObjectId $sqlServer.Identity.PrincipalId.Guid `
    -RoleDefinitionName 'Storage Blob Data Contributor' `
    -ResourceName $StorageAccountName `
    -ResourceType 'Microsoft.Storage/storageAccounts' `
    -ResourceGroupName $ResourceGroupName
```
![image](https://user-images.githubusercontent.com/96930989/228438226-f231b96b-4f91-4014-ad3b-a64a61a40782.png)


##### Enable Advanced Data Security
```powershell
Enable-AzSqlServerAdvancedDataSecurity -DoNotConfigureVulnerabilityAssessment -ResourceGroupName $ResourceGroupName -ServerName $ServerName
```


##### Set VA policy with an empty SAS Key, this indicates we are using Managed Identity
```powershell
Update-AzSqlServerVulnerabilityAssessmentSetting `
    -ResourceGroupName $ResourceGroupName `
    -ServerName $ServerName `
    -BlobStorageSasUri "https://$StorageAccountName.blob.core.windows.net/vulnerability-assessment?" `
    -RecurringScansInterval Weekly
```

Then we go to the portal, at that point we can modify the storage account we want to use in different resource groups

![image](https://user-images.githubusercontent.com/96930989/228439099-9a3cc4a4-500b-4b9f-a5ac-b0166609538a.png)

