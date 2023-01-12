## MMA Troubleshooting

### Issue 1 : 
#### The MMA extension handler is showing Not-Ready status and reports the below error, though the extension is Provisioning succeeded.
![image](https://user-images.githubusercontent.com/96930989/212014226-b2b15436-36cc-4034-ba1c-3009f461c21f.png)

"The agent could not connect to the Microsoft Operations Management Suite service. Please check that the system either has Internet access, or that a valid HTTP proxy has been configured for the agent"

#### TSG steps on Windows machine
1. Once login on the VM, go to `Control Panel -> All Control Panel Items` and here you could see `Microsoft Monitoring Agent`, click on it to bring up the UI.
![image](https://user-images.githubusercontent.com/96930989/212015109-39f05712-ff70-416d-825b-4b2ce4b4bf82.png)


2. Then move to the tab named "Azure Log Analytics" and please kindly verify if the workspace id is correct, and if the Status column of this workspace shows green checkbox. 

![image](https://user-images.githubusercontent.com/96930989/212016538-d5f340f2-aef0-40b9-857b-6e5a99112199.png)

3. Please kindly identify network connectivity issue by running the `TestCloudConnectivity` tool. The tool is installed `by default` with the agent in the folder `%SystemRoot%\Program Files\Microsoft Monitoring Agent\Agent`

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

