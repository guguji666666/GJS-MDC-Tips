# MDC- useful information about workspaces and agents
## Defender for servers - worksapces and agents
### 1. The behavior that defender for cloud create workspaces
[Defender for server creates workspaces](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-defender-for-servers-data-workspace#default-workspace)

### 2. Where is the default Log Analytics workspace created?
[Locations of default workspaces](https://learn.microsoft.com/en-us/azure/defender-for-cloud/faq-data-collection-agents#where-is-the-default-log-analytics-workspace-created-)

### 3. Can I delete the default workspaces created by Defender for Cloud?
[Delete default workspaces?](https://learn.microsoft.com/en-us/azure/defender-for-cloud/faq-data-collection-agents#can-i-delete-the-default-workspaces-created-by-defender-for-cloud-)

Deleting the default workspace is not recommended. Defender for Cloud uses the default workspaces to store security data from your VMs. If you delete a workspace, Defender for Cloud is unable to collect this data and some security recommendations and alerts are unavailable.

To recover, remove the Log Analytics agent on the VMs connected to the deleted workspace. Defender for Cloud reinstalls the agent and creates new default workspaces.
![image](https://user-images.githubusercontent.com/96930989/210910915-7ee87ba4-bfc7-442b-8863-5963ca7aa6dc.png)

### 4. What if the Log Analytics agent was already installed as an extension on the VM?
[Extension already installed before enabling auto-provisioning?](https://learn.microsoft.com/en-us/azure/defender-for-cloud/faq-data-collection-agents#what-if-the-log-analytics-agent-was-already-installed-as-an-extension-on-the-vm-)

### 5. Delete and recover Azure Log Analytics workspace
[Delete and recover Azure Log Analytics workspace](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/delete-workspace)

## Useful Azure initiatives/policies
[Auto provisioing - Deploy MMA extension and report to default workspace](https://portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F6df2fee6-a9ed-4fef-bced-e13be1b25f1c)

[Auto provisioing - Deploy MMA extension and report to custom workspace](https://portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F8e7da0a5-0a0e-4bbc-bfc0-7773c018b616)

[Auto provisioing - Deploy AMA extension on Azure VM running Windows](https://portal.azure.com/#view/Microsoft_Azure_Policy/InitiativeDetailBlade/id/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2F9575b8b7-78ab-4281-b53b-d3c1ace2260b/scopes~/%5B%22%2Fsubscriptions%2F1c3c69b0-671e-4dd2-861e-57df09670ef8%22%2C%22%2Fsubscriptions%2Fd4a6045b-9c10-4581-83fe-25d47c5f592e%22%2C%22%2Fsubscriptions%2F74a72629-ac6d-44db-a66a-abc69f3bfb7e%22%5D)

[Auto provisioing - Deploy AMA extension on Azure VM running Linux](https://portal.azure.com/#view/Microsoft_Azure_Policy/InitiativeDetailBlade/id/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2F118f04da-0375-44d1-84e3-0fd9e1849403/scopes~/%5B%22%2Fsubscriptions%2F1c3c69b0-671e-4dd2-861e-57df09670ef8%22%2C%22%2Fsubscriptions%2Fd4a6045b-9c10-4581-83fe-25d47c5f592e%22%2C%22%2Fsubscriptions%2F74a72629-ac6d-44db-a66a-abc69f3bfb7e%22%5D)

## Manually deploy MMA/AMA extension via Powershell/Azure CLI
[Deploy MMA extension on Windows machines](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/oms-windows?toc=%2Fazure%2Fazure-monitor%2Ftoc.json#powershell-deployment)

[Deploy MMA extension on Linux machines](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/oms-linux?toc=%2Fazure%2Fazure-monitor%2Ftoc.json#azure-cli-deployment)

[Deploy AMA extension](https://learn.microsoft.com/en-us/azure/azure-monitor/agents/azure-monitor-agent-manage?tabs=azure-powershell#install)
