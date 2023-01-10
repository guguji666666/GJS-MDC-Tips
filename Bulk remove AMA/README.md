## Remove AMA from multiple VM using powershell

### For Azure VM running `Windows OS`
```powershell
Set-ExecutionPolicy RemoteSigned

Install-Module Az
  
Connect-AzAccount -Subscription <subscription id>

$para = Get-AzVM -ResourceGroupName <resource group name> 
foreach ($Name in $para.name)
{Remove-AzVMExtension -Name AzureMonitorWindowsAgent -ResourceGroupName <resource group name> -VMName $Name -Force} 
```

### For Azure VM running `Linux`
```powershell
Set-ExecutionPolicy RemoteSigned

Install-Module Az
  
Connect-AzAccount -Subscription <subscription id>

$para = Get-AzVM -ResourceGroupName <resource group name> 
foreach ($Name in $para.name)
{Remove-AzVMExtension -Name AzureMonitorLinuxAgent -ResourceGroupName <resource group name> -VMName $Name -Force}
```

You will see the results below once the operation has been performed successfully on the machines
![image](https://user-images.githubusercontent.com/96930989/211447888-4db1d32e-3ee7-42d5-adcd-9405f23d3fea.png)

#### Other reference

[Get-AzVM](https://learn.microsoft.com/en-us/powershell/module/az.compute/get-azvm?view=azps-9.2.0)

[Manage AMA on Azure VM or Arc-enabled VM](https://learn.microsoft.com/en-us/azure/azure-monitor/agents/azure-monitor-agent-manage?tabs=azure-powershell#uninstall)

[Details of defender for server plan](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-defender-for-servers-data-workspace)

[Deploy MMA or AMA to defined scope using azure policy](https://learn.microsoft.com/en-us/azure/azure-monitor/policy-reference)
