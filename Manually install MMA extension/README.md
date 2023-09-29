# Manually install MMA extension on `Azure VM`
## Notice on MMA deprecation
The Log Analytics agent is on a `deprecation path` and won't be supported after `August 31, 2024`. If you use the Log Analytics agent to ingest data to Azure Monitor, migrate to the new Azure Monitor agent prior to that date.
* [Log Analytics agent overview](https://learn.microsoft.com/en-us/azure/azure-monitor/agents/log-analytics-agent)
* [MMA/AMA supported OS](https://learn.microsoft.com/en-us/azure/azure-monitor/agents/agents-overview#supported-operating-systems)
* [Multi-homing in MMA and AMA](https://learn.microsoft.com/en-us/azure/sentinel/ama-migrate#gap-analysis-between-agents)
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/a22ed3d2-6c9c-4947-aec5-e76fe69b9665) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/c2378eef-8c12-4e86-b12c-5c1704543970)

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
### 1. Manually deploy MMA extension on Arc-enabled VM running `Windows`
We can refer to the doc [Virtual machine extension management with Azure Arc-enabled servers](https://learn.microsoft.com/en-us/azure/azure-arc/servers/manage-vm-extensions) <br>
We can deploy the MMA agent using Azure CLI command folowing the steps in [Enable extension on Arc VM](https://learn.microsoft.com/en-us/azure/azure-arc/servers/manage-vm-extensions-cli#enable-extension)
```sh
az connectedmachine extension create --machine-name "myMachineName" --name "MicrosoftMonitoringAgent" --location "regionName" --settings '{\"workspaceId\":\"myWorkspaceId\"}' --protected-settings '{\"workspaceKey\":\"myWorkspaceKey\"}' --resource-group "myResourceGroup" --type-handler-version "1.13" --type "OmsAgentForLinux or MicrosoftMonitoringAgent" --publisher "Microsoft.EnterpriseCloud.Monitoring"
```

### 2. Manually deploy MMA extension on Arc-enabled VM running `Linux`
We can refer to the doc [Install Linux OMS agent](https://learn.microsoft.com/en-us/azure/azure-monitor/agents/agent-windows?tabs=command-line#install-the-agent)<br>
Run the command below for installtion <br>
```sh
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
```

## Purge OMS and reinstallation
```powershell
## Purge existing oms agent

wget https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/tools/purge_omsagent.sh

sudo sh purge_omsagent.sh

## Reinstall oms agent with latest version

wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>

## Verify the version of OMS agent
sudo sh ./omsagent-*.universal.x64.sh --version
```

