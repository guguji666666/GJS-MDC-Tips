## [Security recommendations - a reference guide](https://learn.microsoft.com/en-us/azure/defender-for-cloud/recommendations-reference)
## [VMSS support](https://learn.microsoft.com/en-us/azure/defender-for-cloud/support-matrix-defender-for-servers#supported-features-for-virtual-machines-and-servers)
![image](https://user-images.githubusercontent.com/96930989/236124943-a5ded4a7-33b1-4d51-8d69-71b999262c9b.png) <br>
![image](https://user-images.githubusercontent.com/96930989/236125002-1e61c91d-efb8-42cb-9826-ea7c89e75d53.png)

# Defender for cloud FAQ

## Defender for servers plan - workspaces and agents
### 1. The behavior that defender for cloud creates `default` workspaces
[Defender for server creates workspaces](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-defender-for-servers-data-workspace#default-workspace)

[Security alerts - a reference guide](https://learn.microsoft.com/en-us/azure/defender-for-cloud/alerts-reference#alerts-dns)

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

### 5. What if a Log Analytics agent is directly installed on the machine but not as an extension (Direct Agent)?

If the Log Analytics agent is installed directly on the VM (not as an Azure extension), Defender for Cloud `will install` the Log Analytics agent extension, and may `upgrade` the Log Analytics agent to the latest version.

The agent installed will `continue` to report to its `already configured workspace(s)`, and `in addition` will report to the workspace `configured in Defender for Cloud` (`Multi-homing` is supported on `Windows` machines).

If the configured workspace is a user workspace (not Defender for Cloud's default workspace), you will need to install the "Security" or "SecurityCenterFree" solution on it for Defender for Cloud to start processing events from VMs and computers reporting to that workspace.

For `Linux` machines, Agent multi-homing is `not yet supported` - hence, if an existing agent installation is detected, automatic provisioning `will not occur` and the machine's configuration will not be altered.

For `existing machines` on subscriptions onboarded to Defender for Cloud `before March 17 2019`, when an existing agent will be detected, the Log Analytics agent extension `will not be` installed and the machine will not be affected. For these machines, see the "Resolve monitoring agent health issues on your machines" recommendation to resolve the agent installation issues on these machines

### 6. Can I enable Defender for Servers on a subset of machines in a subscription?
As mentioned in [Defender for cloud FAQ](https://learn.microsoft.com/en-us/azure/defender-for-cloud/faq-defender-for-servers#can-i-enable-defender-for-servers-on-a-subset-of-machines-in-a-subscription-), when you enable Microsoft Defender for Servers on an Azure subscription or on a connected AWS account or GCP project, all connected machines are protected by Defender for Servers. Servers that don't have the Log Analytics agent or Azure Monitor agent installed are also protected. <br>
However, as mentioned in [Plan your Defender for Servers deployment](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-defender-for-servers), you can enable Microsoft Defender for Servers at the `Log Analytics workspace level`, but only servers reporting to that workspace will be protected and billed and those servers won't receive some benefits, such as `Microsoft Defender for Endpoint, vulnerability assessment, and just-in-time VM access`.

To enable defender for server on the workspace, <br>
Navigate to defender for cloud > Environment settings, select the workspace you want to configure <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/7dc97b26-d671-416b-a352-da03ade14ec9) <br>
In `Settings | Defender plans` page, turn on the switch of Servers, save the settings <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/0d9c4ce4-da51-4370-aed5-dc2d60725bf0)

### 7. Billing after enable defender for server on subscription and workspace level?
[Billing for defender for server](https://learn.microsoft.com/en-us/azure/defender-for-cloud/faq-defender-for-servers#do-i-need-to-enable-on-the-subscription-and-workspace-)

When you enable the Servers plan on the `subscription level`, Defender for Cloud enables the plan on your `default workspaces` automatically. 

If you're using a `custom workspace`, you need to select it to enable the plan manually. 
![image](https://user-images.githubusercontent.com/96930989/211142147-1d9f5d8c-4bd8-4ba5-bd9f-37c065e20fe1.png)

![image](https://user-images.githubusercontent.com/96930989/211142168-0fddb5ae-3321-4a2a-98ad-1475cc4ce73b.png)

### Notice !!!

If you turn on Defender for Servers for a subscription and for a connected custom workspace, you `aren't charged` for both. The system identifies `unique VMs`.

If you enable Defender for Servers on cross-subscription workspaces:
* For the `Log Analytics agent`, connected machines from all subscriptions are billed, including subscriptions that don't have the servers plan enabled.
* For the `Azure Monitor agent`, billing and feature coverage for Defender for Servers depends only on the plan being enabled in the subscription.

### Reference
* [If I enable Defender for Clouds Servers plan on the subscription level, do I need to enable it on the workspace level?](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-defender-for-servers-data-workspace#if-i-enable-defender-for-clouds-servers-plan-on-the-subscription-level-do-i-need-to-enable-it-on-the-workspace-level)
* [Do I need to enable Defender for Servers on the subscription and on the workspace?](https://learn.microsoft.com/en-us/azure/defender-for-cloud/faq-defender-for-servers#do-i-need-to-enable-defender-for-servers-on-the-subscription-and-on-the-workspace-)

### 8. How to check which recommendation affects the secure score?
Enable continuous export together with the built-in workbook so that you can get more details about how recommendations affect your secure score. <br>
[Continuously export Microsoft Defender for Cloud data](https://learn.microsoft.com/en-us/azure/defender-for-cloud/continuous-export?tabs=azure-portal)

Built-in workbook <br>
![image](https://user-images.githubusercontent.com/96930989/214222344-8a076879-cb63-4024-a2bf-6fabcc5539b4.png)

Sample <br>
![image](https://user-images.githubusercontent.com/96930989/214222646-77c9f727-1256-459e-82f8-a6e6d5a603c8.png)


### 9. Defender for Cloud supported OS

[Defender for cloud supported OS](https://learn.microsoft.com/en-us/azure/defender-for-cloud/support-matrix-defender-for-cloud#supported-operating-systems)

![image](https://guguimage.aceultraman.com/i/2023/05/20/ey8ein.png)

[MMA,AMA supported OS](https://learn.microsoft.com/en-us/azure/azure-monitor/agents/agents-overview#supported-operating-systems)
![image](https://guguimage.aceultraman.com/i/2023/05/20/ezlkwz.png)
 
AMA is not supported for Windows server 2008, 2008R2 <br>
AMA is supported for Windows server 2012, 2012R2

### 10. Will Defender for server cover Windows client machine?
1. Create Azure VM for test, OS information <br>
![image](https://guguimage.aceultraman.com/i/2023/05/20/e6baqy.png)

2. AMA is installed via auto-provisioning <br>
![image](https://guguimage.aceultraman.com/i/2023/05/20/e5q4zr.png)

3. Win10 Pro client OS is supported by defender for cloud <br>
![image](https://guguimage.aceultraman.com/i/2023/05/20/fnv6j1.png)

4. The machine is protected by defender for cloud , we can confirm it in `defender for cloud > inventory` <br>
![image](https://guguimage.aceultraman.com/i/2023/05/20/flh68q.png)


### 11. Delete and recover Azure Log Analytics workspace (Reach Azure monitoring team)
[Delete and recover Azure Log Analytics workspace](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/delete-workspace)



