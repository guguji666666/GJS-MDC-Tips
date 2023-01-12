## MMA Troubleshooting

### Issue 1 : 
#### The MMA extension handler is showing Not-Ready status and reports the below error, though the extension is Provisioning succeeded.
![image](https://user-images.githubusercontent.com/96930989/212014226-b2b15436-36cc-4034-ba1c-3009f461c21f.png)

"The agent could not connect to the Microsoft Operations Management Suite service. Please check that the system either has Internet access, or that a valid HTTP proxy has been configured for the agent"

#### TSG steps on Windows machine (Whether MMA is installed as extension or MSI package)
1. RDP to the VM, go to `Control Panel -> System and Security` and here you could see `Microsoft Monitoring Agent`, click it.
![image](https://user-images.githubusercontent.com/96930989/212033799-9fb7eec1-4179-4de4-8c7f-901c709694c8.png)

2. Move to the tab "Azure Log Analytics" and verify if the workspace id is correct, and if the Status of this workspace shows green checkbox.
![image](https://user-images.githubusercontent.com/96930989/212016538-d5f340f2-aef0-40b9-857b-6e5a99112199.png)

3. Identify network connectivity issue by running the `TestCloudConnectivity` tool. The tool is installed `by default` with the agent in the folder `%SystemRoot%\Program Files\Microsoft Monitoring Agent\Agent`

Launch cmd with administrator, then run the commands below:
```sh
cd C:\Program Files\Microsoft Monitoring Agent\Agent
```
```sh
.\testcloudconnection
```
You will get the result jist like the sample below
![image](https://user-images.githubusercontent.com/96930989/212016966-8b89a925-14c7-4082-b609-ae0c12f9a654.png)

Check the results and then modify the configurations accordingly.

Here’s the [Firewall requirements](https://learn.microsoft.com/en-us/azure/azure-monitor/agents/log-analytics-agent#firewall-requirements) for MMA to function well for your reference
![image](https://user-images.githubusercontent.com/96930989/212018673-5ebfbe33-b874-446e-955d-e1c93c8dcfc0.png)

