# MDC- useful information about workspaces and agents
## Defender for servers - workspaces and agents
### 1. The behavior that defender for cloud create workspaces
[Defender for server creates workspaces](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-defender-for-servers-data-workspace#default-workspace)

By default, when you onboard for the first time Defender for Cloud creates a new resource group and default workspace in the region of each subscription with Defender for Cloud enabled.

If you have `VMs` in `multiple locations`, Defender for Cloud creates `multiple workspaces` accordingly, to ensure data compliance.

### 2. Where is the default Log Analytics workspace created?
[Locations of default workspaces](https://learn.microsoft.com/en-us/azure/defender-for-cloud/faq-data-collection-agents#where-is-the-default-log-analytics-workspace-created-)
```
For VMs in the United States and Brazil the workspace location is the United States
For VMs in Canada, the workspace location is Canada
For VMs in Europe the workspace location is Europe
For VMs in the UK the workspace location is the UK
For VMs in East Asia and Southeast Asia the workspace location is Asia
For VMs in Korea, the workspace location is Korea
For VMs in India, the workspace location is India
For VMs in Japan, the workspace location is Japan
For VMs in China, the workspace location is China
For VMs in Australia, the workspace location is Australia
```

### 3. Can I delete the default workspaces created by Defender for Cloud?
[Delete default workspaces?](https://learn.microsoft.com/en-us/azure/defender-for-cloud/faq-data-collection-agents#can-i-delete-the-default-workspaces-created-by-defender-for-cloud-)

Deleting the default workspace is not recommended. Defender for Cloud uses the default workspaces to store security data from your VMs. If you delete a workspace, Defender for Cloud is unable to collect this data and some security recommendations and alerts are unavailable.

To recover, remove the Log Analytics agent on the VMs connected to the deleted workspace. Defender for Cloud reinstalls the agent and creates new default workspaces.
![image](https://user-images.githubusercontent.com/96930989/210910915-7ee87ba4-bfc7-442b-8863-5963ca7aa6dc.png)

### 4. What if the Log Analytics agent was already installed as an extension on the VM?
[Extension already installed before enabling auto-provisioning?](https://learn.microsoft.com/en-us/azure/defender-for-cloud/faq-data-collection-agents#what-if-the-log-analytics-agent-was-already-installed-as-an-extension-on-the-vm-)

### 5. Billing once after enable defender for server on subscription and workspace level?
[Billing for defender for server](https://learn.microsoft.com/en-us/azure/defender-for-cloud/faq-defender-for-servers#do-i-need-to-enable-on-the-subscription-and-workspace-)

### 6. Delete and recover Azure Log Analytics workspace (Azure monitoring team)
[Delete and recover Azure Log Analytics workspace](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/delete-workspace)

## Deploy Auto-provisioning 

#### [Scaling auto-provisioning using Azure policy](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-defender-for-servers-scale#scaling-auto-provisioning)

[Auto provisioing - Deploy MMA extension and report to default workspace](https://portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F6df2fee6-a9ed-4fef-bced-e13be1b25f1c)

[Auto provisioing - Deploy MMA extension and report to custom workspace](https://portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F8e7da0a5-0a0e-4bbc-bfc0-7773c018b616)

[[Preview]: Configure Arc machines to create the default Microsoft Defender for Cloud pipeline using AMA](https://portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3b1a8e0a-b2e1-48be-9365-28be2fbef550)

[[Preview]: Configure virtual machines to create the default Microsoft Defender for Cloud pipeline using AMA](https://portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F8b5ad9ab-3d44-4a6e-9ac3-75b04ea5fd28)

[[Preview]: Configure Arc machines to create the Microsoft Defender for Cloud user-defined pipeline using AMA](https://portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Faba46665-c3a7-4319-ace1-a0282deebac2)

[[Preview]: Configure machines to create the Microsoft Defender for Cloud user-defined pipeline using AMA](https://portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc15c5978-ab6e-4599-a1c3-90a7918f5371)

## Manually deploy MMA/AMA extension via Powershell/Azure CLI
[Deploy MMA extension on Windows machines](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/oms-windows?toc=%2Fazure%2Fazure-monitor%2Ftoc.json#powershell-deployment)

[Deploy MMA extension on Linux machines](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/oms-linux?toc=%2Fazure%2Fazure-monitor%2Ftoc.json#azure-cli-deployment)

[Deploy AMA extension](https://learn.microsoft.com/en-us/azure/azure-monitor/agents/azure-monitor-agent-manage?tabs=azure-powershell#install)
