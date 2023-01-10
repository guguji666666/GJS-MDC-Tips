# MDC- FAQ
## Defender for servers plan - workspaces and agents
### 1. The behavior that defender for cloud creates workspaces
[Defender for server creates workspaces](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-defender-for-servers-data-workspace#default-workspace)

By default, when you onboard for the first time Defender for Cloud creates a new resource group and default workspace in the region of each subscription with Defender for Cloud enabled.

If you have `VMs` in `multiple locations`, Defender for Cloud creates `multiple workspaces` accordingly, to ensure data compliance.

### 2. Where is the default Log Analytics workspace created by defender for cloud plans?
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

Deleting the default workspace is not recommended if you don't have custom workspace. 

If Defender for cloud is using default workspace but you delete it accidently, Defender for Cloud is unable to collect this data and some security recommendations and alerts are unavailable.

To recover, `remove the Log Analytics agent` on the VMs connected to the deleted workspace. Defender for Cloud `reinstalls the agent` and `creates new default workspaces`. You can also define `custom workspace` in `auto-provisioning` configuration if you don't want MDC to use default workspaces.

![image](https://user-images.githubusercontent.com/96930989/211142697-18ee00ae-5b1d-4668-b95b-068658c6aff0.png)

Move to `on`
![image](https://user-images.githubusercontent.com/96930989/211142730-62f233c4-17c4-4d43-afb2-e63964701883.png)

![image](https://user-images.githubusercontent.com/96930989/211142677-6fef576e-cb30-4106-b20a-62ef814f1384.png)


### 4. What if the Log Analytics agent was already installed as an extension on the VM?
[Extension already installed before enabling auto-provisioning?](https://learn.microsoft.com/en-us/azure/defender-for-cloud/faq-data-collection-agents#what-if-the-log-analytics-agent-was-already-installed-as-an-extension-on-the-vm-)

When the Monitoring Agent has already been `installed as an extension`(such as manual installation via `API`), the extension configuration allows reporting to `only a single workspace`. 

Defender for Cloud `does not` override `existing connections` to user workspaces. 

Defender for Cloud will store security data from a VM in a workspace that is `already connected`, provided that the "Security" or "SecurityCenterFree" solution has been installed on it. 

Defender for Cloud may `upgrade the extension version` to the latest version in this process.

### 5. Billing once after enable defender for server on subscription and workspace level?
[Billing for defender for server](https://learn.microsoft.com/en-us/azure/defender-for-cloud/faq-defender-for-servers#do-i-need-to-enable-on-the-subscription-and-workspace-)

When you enable the Servers plan on the `subscription level`, Defender for Cloud enables the plan on your `default workspaces` automatically. 

If you're using a `custom workspace`, you need to select it to enable the plan manually. 
![image](https://user-images.githubusercontent.com/96930989/211142147-1d9f5d8c-4bd8-4ba5-bd9f-37c065e20fe1.png)

![image](https://user-images.githubusercontent.com/96930989/211142168-0fddb5ae-3321-4a2a-98ad-1475cc4ce73b.png)

Note that:

If you turn on Defender for Servers for a subscription and for a connected custom workspace, you `aren't charged` for both. The system identifies `unique VMs`.

If you enable Defender for Servers on cross-subscription workspaces:
* For the `Log Analytics agent`, connected machines from all subscriptions are billed, including subscriptions that don't have the servers plan enabled.
* For the `Azure Monitor agent`, billing and feature coverage for Defender for Servers depends only on the plan being enabled in the subscription.

### 6. Delete and recover Azure Log Analytics workspace (Azure monitoring team)
[Delete and recover Azure Log Analytics workspace](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/delete-workspace)

### Deploy Auto-provisioning 

#### [Scaling auto-provisioning using Azure policy](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-defender-for-servers-scale#scaling-auto-provisioning)

[Auto provisioing - Deploy MMA extension and report to default workspace](https://portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F6df2fee6-a9ed-4fef-bced-e13be1b25f1c)

[Auto provisioing - Deploy MMA extension and report to custom workspace](https://portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F8e7da0a5-0a0e-4bbc-bfc0-7773c018b616)

[[Preview]: Configure Arc machines to create the default Microsoft Defender for Cloud pipeline using AMA](https://portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3b1a8e0a-b2e1-48be-9365-28be2fbef550)

[[Preview]: Configure virtual machines to create the default Microsoft Defender for Cloud pipeline using AMA](https://portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F8b5ad9ab-3d44-4a6e-9ac3-75b04ea5fd28)

[[Preview]: Configure Arc machines to create the Microsoft Defender for Cloud user-defined pipeline using AMA](https://portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Faba46665-c3a7-4319-ace1-a0282deebac2)

[[Preview]: Configure machines to create the Microsoft Defender for Cloud user-defined pipeline using AMA](https://portal.azure.com/#view/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc15c5978-ab6e-4599-a1c3-90a7918f5371)

### Manually deploy MMA/AMA extension via Powershell/Azure CLI
[Deploy MMA extension on Windows machines](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/oms-windows?toc=%2Fazure%2Fazure-monitor%2Ftoc.json#powershell-deployment)

[Deploy MMA extension on Linux machines](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/oms-linux?toc=%2Fazure%2Fazure-monitor%2Ftoc.json#azure-cli-deployment)

[Deploy AMA extension](https://learn.microsoft.com/en-us/azure/azure-monitor/agents/azure-monitor-agent-manage?tabs=azure-powershell#install)

## Extension for VA scanning
### 1. What is `AzSecPack`?

First, we have the concept `off-Node Service`

The `Off-Node Service (ONS)` is an agent-based vulnerability scanning solution provided by the C+AI organization to identify publicly disclosed vulnerabilities in the underlying OS (Windows, Linux), OS features (IIS, SQL Server) and 3rd-party software (Adobe, Wireshark).  Off-Node satisfies individual service teams, as well as Microsoft's overall Azure compliance and security requirements for Risk Assessment controls.

Off-Node is fully automated and is `delivered automatically` by `AzSecPack`. Broad and comprehensive deployment of AzSecPack is a `prerequisite` for Off-Node scanning.

Together, AzSecPack and Off-Node provide all the needed compliance audits and controls required for regulatory compliance.

AzSecPack is responsible for monitoring the vulnerabilities and managing the security of the Azure infrastructure.


You may find the AZSecPack on `Linux hosts` or `AKS agent nodes`. 

Supported OS Versions:
```
Ubuntu 14.04, 16.04, 17.10, & 18.04 Debian 7, 8, & 9 CentOS 6 & 7
RHEL 6 & 7
Oracle Linux 6 & 7
SLES 12
```

All in all, the AzSecPack could be considered as a component to improve VA scanning and enables true security monitoring of your service.


