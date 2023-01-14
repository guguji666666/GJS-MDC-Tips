## Qualys VA scanning troubleshooting

### Workflow of qualys agent
[Defender for Cloud's integrated Qualys vulnerability scanner for Azure and hybrid machines](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-vm)
![image](https://user-images.githubusercontent.com/96930989/212463315-f45920c2-7977-4350-9b55-985fe84b0931.png)

1. Deploy - Microsoft Defender for Cloud monitors your machines and provides recommendations to deploy the Qualys extension on your selected machine/s.

2. Gather information - The extension collects artifacts and sends them for analysis in the Qualys cloud service in the defined region.

3. Analyze - Qualys' cloud service conducts the vulnerability assessment and sends its findings to Defender for Cloud.

4. Report - The findings are available in Defender for Cloud.

Note
```
Qualys vulnerability assessment is performed in the Qualys Cloud by analysing metadata that is uploaded from the agent to Qualys.

Vulnerability assessment results are synced between Qualys and Azure Security Center every 4 hours. 

As the metadata scan and upload only occurs every 12 hours, there can be a delay in seeing remediations reflected in Defender for cloud.
```

[Qualys supported OS](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-vm#why-does-my-machine-show-as-not-applicable-in-the-recommendation)

![image](https://user-images.githubusercontent.com/96930989/212463200-28dfd795-2b93-40e9-ab37-61e3161dc64d.png)


### Azure VM running Windows TSG steps
##### 1. Check the installation of the qualys extension
##### 2. Test connection to qualys cloud service

[What prerequisites and permissions are required to install the Qualys extension?](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-vm#what-prerequisites-and-permissions-are-required-to-install-the-qualys-extension)

• https://qagpublic.qg3.apps.qualys.com - Qualys' US data center

• https://qagpublic.qg2.apps.qualys.eu - Qualys' European data center
	
If your machine is in a region in an Azure `European geography` (such as Europe, UK, Germany), its artifacts will be processed in Qualys' `European data center`. 

Artifacts for virtual machines located `elsewhere` are sent to the `US data center`.

##### 3. Check if the metadata is generated
Navigate to the path below and see if `Vulnerability_snapshot.db` exists
```
C:\ProgramData\Qualys\QualysAgent\Vulnerability_snapshot.db
```
![image](https://user-images.githubusercontent.com/96930989/212463493-d5981a23-051a-4ca0-b561-6ce8e9cca92e.png)
##### 4. Perform on demand scan, then wait for 24 hours
[Trigger an on-demand scan](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-vm#trigger-an-on-demand-scan)
```cmd
REG ADD HKLM\SOFTWARE\Qualys\QualysAgent\ScanOnDemand\Vulnerability /v "ScanOnDemand" /t REG_DWORD /d "1" /f
```
##### 5. If the issue still persists, reinstall the qualys agent, then wait for 24 hours
##### 6. Collect the logs and reach Microsoft support


### Azure VM running Linux TSG steps
##### 1. Check the installation of the qualys extension
##### 2. Test connection to qualys cloud service
##### 3. Check if the metadata is generated
Navigate to the path below and see if `SnapshotVM.db` exists
```
/usr/local/qualys/cloud-agent/SnapshotVM.db
```
![image](https://user-images.githubusercontent.com/96930989/212463514-a666a0cd-8b79-448c-ae3e-27ae47d67960.png)
##### 5. Perform on demand scan, then wait for 24 hours
[Trigger an on-demand scan](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-vm#trigger-an-on-demand-scan)
```sh
sudo /usr/local/qualys/cloud-agent/bin/cloudagentctl.sh action=demand type=vm
```
##### 6. If the issue still persists, reinstall the qualys agent, then wait for 24 hours
##### 7. Collect the logs and reach Microsoft support
