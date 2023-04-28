## Recommendation "Endpoint protection should be installed on machines"

### The VM will be considered as `Healthy resource` when they meet the requirements below:
1. Defender for server plan 1/2 is enabled at `subscription level`
2. Defender for server plan also enabled at `workspace level`
3. MMA (Microsoft monitoring agent) or AMA (Azure monitoring agent) is installed on the VM and `provisioned successfully`
4. 3rd party endpoint protection solution supported by MS is installed [Supported endpoint protection solutions](https://learn.microsoft.com/en-us/azure/defender-for-cloud/supported-machines-endpoint-solutions-clouds-servers?tabs=features-windows#supported-endpoint-protection-solutions)
5. VM must be in `running` state when the recommendation refreshes the results (better to run 24*7)
6. MMA or AMA is reporting to the workspace (results should be found from `heartbeat`)

![image](https://user-images.githubusercontent.com/96930989/211691888-58efdbf5-c84b-44c3-8cf6-2bdfb3596146.png)

7. The query `ProtectionStatus` should return the results

![image](https://user-images.githubusercontent.com/96930989/211691952-851a9ff2-5443-4777-bfd4-0bcb0d861db1.png)

### Endpoint protection solutions
[Endpoint protection assessment and recommendations in Microsoft Defender for Cloud](https://learn.microsoft.com/en-us/azure/defender-for-cloud/endpoint-protection-recommendations-technical#windows-defender)

#### Windows Defender
1. Defender for Cloud recommends Endpoint protection should be installed on your machines when `Get-MpComputerStatus` runs and the result is `AMServiceEnabled: False`
2. Defender for Cloud recommends Endpoint protection health issues should be resolved on your machines when `Get-MpComputerStatus` runs and any of the following occurs:
Any of the following properties are false:
* AMServiceEnabled
* AntispywareEnabled
* RealTimeProtectionEnabled
* BehaviorMonitorEnabled
* IoavProtectionEnabled
* OnAccessProtectionEnabled

If one or both of the following properties are 7 or more:
* AntispywareSignatureAge
* AntivirusSignatureAge

Sample output from healthy resources (Window server 2019 + Arc)
![image](https://user-images.githubusercontent.com/96930989/211692355-9bcfde75-3203-4b1c-a8e4-6809f13cea66.png)
