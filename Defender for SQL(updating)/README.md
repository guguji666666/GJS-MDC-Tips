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
* Apply to configuration at the server side (defender for SQL should be enabled at server level)
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
