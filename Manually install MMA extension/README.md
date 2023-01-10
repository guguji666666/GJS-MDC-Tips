## Manually MMA extension on `Azure VM`

### 1. Manually MMA extension on Azure VM running `Windows`
[MMA extension for Windows](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/oms-windows?toc=%2Fazure%2Fazure-monitor%2Ftoc.json#powershell-deployment)

[Switch subscription in Azure CLI](https://learn.microsoft.com/en-us/cli/azure/manage-azure-subscriptions-azure-cli#change-the-active-subscription)

[Set-AzVMExtension](https://learn.microsoft.com/en-us/powershell/module/az.compute/set-azvmextension?view=azps-9.2.0)
Make sure your Azure VM is in `running` state

Using Azure CLI
```powershell

Set-ExecutionPolicy RemoteSigned

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
![image](https://user-images.githubusercontent.com/96930989/211567930-f29da061-18f6-4556-8368-5c6cce4f24f0.png)

Sample
![image](https://user-images.githubusercontent.com/96930989/211565864-5cd46c5a-38d4-4590-813b-f440ed91d16e.png)

![image](https://user-images.githubusercontent.com/96930989/211569162-c3c003b3-cc81-408d-afeb-cc6834b812b5.png)


### 2. Manually MMA extension on Azure VM running `Linux`
[MMA extension for Linux](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/oms-linux?toc=%2Fazure%2Fazure-monitor%2Ftoc.json)

[Switch subscription in Azure CLI](https://learn.microsoft.com/en-us/cli/azure/manage-azure-subscriptions-azure-cli#change-the-active-subscription)

Make sure your Azure VM is in `running` state

Using Azure CLI
```powershell
az vm extension set --resource-group <name of the rg where VM belongs to> --vm-name <name of VM> --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --protected-settings '{"workspaceKey":"myWorkspaceKey"}' --settings '{"workspaceId":"myWorkspaceId","skipDockerProviderInstall": true}' --version 1.13
```
Sample

![image](https://user-images.githubusercontent.com/96930989/211572995-01220bed-d43f-4b73-8914-7abf208cd09c.png)

![image](https://user-images.githubusercontent.com/96930989/211573097-676a1207-d0cf-46b0-b7c7-5d5403d979a7.png)


## Manually MMA extension on `Arc-enabled VM`
### 1. Manually MMA extension on Arc-enabled VMrunning Windows

### 2. Manually MMA extension on Arc-enabled VM running Linux
