# Qualys VA scanning troubleshooting

## Workflow of qualys agent
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

