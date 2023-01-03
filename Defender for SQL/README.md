# Microsoft Defender for Azure SQL
```
Overview of Microsoft Defender for Azure SQL
https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-sql-introduction

Find and remediate vulnerabilities in your Azure SQL databases
https://learn.microsoft.com/en-us/azure/defender-for-cloud/sql-azure-vulnerability-assessment-find?tabs=express
```
## SQL managed instance - Define Storage account before enabling defender for SQL plan (server side) 

```
Reference for powershell commands
https://learn.microsoft.com/en-us/powershell/module/az.sql/enable-azsqlinstanceadvanceddatasecurity?view=azps-9.2.0
https://learn.microsoft.com/en-us/powershell/module/az.sql/update-azsqlinstancedatabasevulnerabilityassessmentsetting?view=azps-9.2.0
```
#### Symptom : User can not perform manual scan after enabling the plan at subscription level
![image](https://user-images.githubusercontent.com/96930989/210368162-e3c1e76a-74a8-4aa7-b223-67d83a639502.png)
![image](https://user-images.githubusercontent.com/96930989/210368172-241a74de-d04f-4dec-a175-3b9c559cacb3.png)
![image](https://user-images.githubusercontent.com/96930989/210368184-cbf9c2db-d944-4154-a269-4c8303e40c3f.png)

#### Powershell commands for workaround (SQL managed instance and storage account should in the `same` resource group)
```powershell
$SubscriptionId = "<subscription id>"
$ResourceGroupName = '<name of the resource group where the storage account locates>'
$InstanceName = '<name of SQL managed instance>'
$DatabaseName = '<name of SQL managed database>'
$StorageAccountName = "<name of the storage account>"
```

```powershell
Connect-AzAccount -Subscription $SubscriptionId
```

```powershell
Enable-AzSqlInstanceAdvancedDataSecurity -DoNotConfigureVulnerabilityAssessment -ResourceGroupName $ResourceGroupName -InstanceName $InstanceName
```

```powershell
Update-AzSqlInstanceDatabaseVulnerabilityAssessmentSetting `
            -ResourceGroupName $ResourceGroupName `
            -InstanceName $InstanceName `
            -DatabaseName $DatabaseName `
            -StorageAccountName $StorageAccountName `
            -ScanResultsContainerName "sql-vulnerability-assessment" `
            -RecurringScansInterval Weekly `
            -EmailAdmins $true `
            -NotificationEmail @("<mailbox>")![image](https://user-images.githubusercontent.com/96930989/210368489-678c1995-da8d-431f-930f-9845020eb586.png)
```

#### Sample
![image](https://user-images.githubusercontent.com/96930989/210369235-d5bf1e54-9c16-4aea-9083-24e34cea65f6.png)

#### Then go back to the instance, the scan button is available
![image](https://user-images.githubusercontent.com/96930989/210369316-d3b4f51b-2e3d-4d14-a87c-8fcbd706a14f.png)
![image](https://user-images.githubusercontent.com/96930989/210369376-da860f3b-5ec2-4145-a0c8-47c1280749a0.png)

#### Check if the VA results are updated in the storage account
![image](https://user-images.githubusercontent.com/96930989/210369397-91ebc999-4ae7-4eaa-bf3f-966cfb7ca4a4.png)

```
Note:
If the defender for SQL plan is enabled at instance side (not in subscription level),
The powershell commands above could also define the storage account before enabling the plan at instance side
```
