# Manually install MMA extension on `Azure VM`
## Notice on MMA deprecation
The Log Analytics agent is on a `deprecation path` and won't be supported after `August 31, 2024`. If you use the Log Analytics agent to ingest data to Azure Monitor, migrate to the new Azure Monitor agent prior to that date.
* [Log Analytics agent overview](https://learn.microsoft.com/en-us/azure/azure-monitor/agents/log-analytics-agent)
* [Multi-homing in MMA and AMA](https://learn.microsoft.com/en-us/azure/sentinel/ama-migrate#gap-analysis-between-agents)
![image](https://user-images.githubusercontent.com/96930989/220602245-b1e2022f-bed6-4c00-af55-c8d994ba5cbc.png)
![image](https://user-images.githubusercontent.com/96930989/220602315-8f271d51-ad18-40ca-91b5-6a92595a87a1.png)

* [MMA supported OS](https://learn.microsoft.com/en-us/azure/azure-monitor/agents/agents-overview#supported-operating-systems)

### 1. Manually deploy MMA extension on Azure VM running `Windows`
[MMA extension for Windows](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/oms-windows?toc=%2Fazure%2Fazure-monitor%2Ftoc.json#powershell-deployment)

[Switch subscription in Azure CLI](https://learn.microsoft.com/en-us/cli/azure/manage-azure-subscriptions-azure-cli#change-the-active-subscription)

[Poweshell : Set-AzVMExtension](https://learn.microsoft.com/en-us/powershell/module/az.compute/set-azvmextension?view=azps-9.2.0)

[Azure CLI : az vm extension Reference](https://learn.microsoft.com/en-us/cli/azure/vm/extension?view=azure-cli-latest)

1. Make sure your Azure VM is in `running` state <br>
2. Run powershell commands on the machine <br>
```powershell
Set-ExecutionPolicy Unrestricted
```
```powershell
Install-Module Az
  
Connect-AzAccount -Subscription <subscription id>

$PublicSettings = @{"workspaceId" = "myWorkspaceId"}
$ProtectedSettings = @{"workspaceKey" = "myWorkspaceKey"}

Set-AzVMExtension -ExtensionName "MicrosoftMonitoringAgent" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
    -ExtensionType "MicrosoftMonitoringAgent" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location <Location of VM>
```

Sample <br>
![image](https://user-images.githubusercontent.com/96930989/211575414-8800a998-4ece-47fc-98d9-ac6eef9c12fa.png) <br>
![image](https://user-images.githubusercontent.com/96930989/211575464-1bef01bc-995e-46a7-b9e3-e4fe78fed93f.png) <br>
![image](https://user-images.githubusercontent.com/96930989/211575553-ffab9ace-1093-4bb7-91a2-75c175ddbac1.png) <br>

To verify on local machine, RDP to the VM, go to `Control Panel -> System and Security` and here you could see `Microsoft Monitoring Agent`  <br>
![image](https://user-images.githubusercontent.com/96930989/212033799-9fb7eec1-4179-4de4-8c7f-901c709694c8.png)  <br>

Then move to the tab named "Azure Log Analytics" andverify if the workspace id is correct, and if the Status column of this workspace shows green checkbox. <br>
![image](https://user-images.githubusercontent.com/96930989/212016538-d5f340f2-aef0-40b9-857b-6e5a99112199.png)


### 2. Manually deploy MMA extension on Azure VM running `Linux`
[MMA extension for Linux](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/oms-linux?toc=%2Fazure%2Fazure-monitor%2Ftoc.json)

[Switch subscription in Azure CLI](https://learn.microsoft.com/en-us/cli/azure/manage-azure-subscriptions-azure-cli#change-the-active-subscription)

Make sure your Azure VM is in `running` state

Using `Azure CLI`
```powershell
az vm extension set --resource-group <name of the rg where VM belongs to> --vm-name <name of VM> --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --protected-settings '{"workspaceKey":"myWorkspaceKey"}' --settings '{"workspaceId":"myWorkspaceId","skipDockerProviderInstall": true}' --version 1.13
```
Sample

![image](https://user-images.githubusercontent.com/96930989/211572995-01220bed-d43f-4b73-8914-7abf208cd09c.png)

![image](https://user-images.githubusercontent.com/96930989/211573097-676a1207-d0cf-46b0-b7c7-5d5403d979a7.png)

Note
```
If you remove the MMA extension from the VM, the MMA client will be removed from the machine as well.
```

# Manually install MMA extension on `Arc-enabled VM`


