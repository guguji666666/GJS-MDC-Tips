# Built-in Qualys integration TSG

## [Workflow of qualys agent](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-vm)
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

## [Qualys supported OS](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-vm#why-does-my-machine-show-as-not-applicable-in-the-recommendation)
![image](https://user-images.githubusercontent.com/96930989/212463200-28dfd795-2b93-40e9-ab37-61e3161dc64d.png)


## Bulk deploy qualys built-in solution
#### [Automate at-scale deployments - Defender for Cloud's integrated Qualys vulnerability scanner](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-vm#automate-at-scale-deployments)
![image](https://user-images.githubusercontent.com/96930989/226186069-0fd15aa9-c321-4e4d-b20a-f6c1edd45e7f.png)

### [Automatically enable a vulnerability assessment solution](https://learn.microsoft.com/en-us/azure/defender-for-cloud/auto-deploy-vulnerability-assessment#automatically-enable-a-vulnerability-assessment-solution)

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/4cd3b611-ee53-488e-91f5-886b78a9dec7)

Check this policy in `Policy | Definitions` <br>
```
Configure machines to receive a vulnerability assessment provider
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/62334999-2821-45ab-8c7c-8b8dc62e92fd)

In the assignment we can select `default` or `mdeTVM` as VA provider <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/ba033593-39c2-4106-8140-ae0cdd87ca9d)

If we select `default`, then built-in qualys VA scanner will be deployed on supported VMs <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/ebc404b1-90fb-462c-b11d-3cbc7d99eae0)

We can find the policy definition id `13ce0167-8ca6-4048-8e6b-f996402e3c1b` in audit logs of the VM <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/2cc19ca9-09ff-4a19-b44e-1481f17c3684)

## [KQL query to list qualys VA findings](https://learn.microsoft.com/en-us/azure/defender-for-cloud/resource-graph-samples?tabs=azure-cli#list-qualys-vulnerability-assessment-results)
```kusto
SecurityResources
| where type == 'microsoft.security/assessments'
| where * contains 'vulnerabilities in your virtual machines'
| summarize by assessmentKey=name //the ID of the assessment
| join kind=inner (
	securityresources
	| where type == 'microsoft.security/assessments/subassessments'
	| extend assessmentKey = extract('.*assessments/(.+?)/.*',1,  id)
) on assessmentKey
| project assessmentKey, subassessmentKey=name, id, parse_json(properties), resourceGroup, subscriptionId, tenantId
| extend description = properties.description,
	displayName = properties.displayName,
	resourceId = properties.resourceDetails.id,
	resourceSource = properties.resourceDetails.source,
	category = properties.category,
	severity = properties.status.severity,
	code = properties.status.code,
	timeGenerated = properties.timeGenerated,
	remediation = properties.remediation,
	impact = properties.impact,
	vulnId = properties.id,
	additionalData = properties.additionalData
```



## Optional
#### If you are using Azure policy for deployment and you only want to deploy qualys extension on VMs with specified `tags`, then
1. Hard-code the tag name and value in the policy rule
```json
"policyRule": {
    "if": {
        "allOf": [{
                "field": "type",
                "in": [
                    "Microsoft.Compute/virtualMachines",
                    "Microsoft.ClassicCompute/virtualMachines"
                ]
            }, {
                "field": "tags['environment']",
                "in": ["prod",”UAT”]
            }
        ]
    },
    "then": {
        "effect": "deployIfNotExists",
```

or

2. If you want to parameterize the tag name and value,
```json
"parameters": {
    "inclusionTagName": {
        "type": "String",
        "metadata": {
            "displayName": "Inclusion Tag Name",
            "description": "Name of the tag to use for including VMs in the scope of this policy. 
        }
    },
    "inclusionTagValue": {
        "type": "Array",
        "metadata": {
            "displayName": "Inclusion Tag Values",
            "description": "Value of the tag to use for including VMs in the scope of this policy. 
        }
    }
},
"policyRule": {
    "if": {
        "allOf": [{
                "field": "type",
                "in": [
                    "Microsoft.Compute/virtualMachines",
                    "Microsoft.ClassicCompute/virtualMachines"
                ]
            }, {
                "field": "[concat('tags[', parameters('inclusionTagName'), ']')]",
                "in": "[parameters('inclusionTagValue')]"
            }
        ]
    },
    "then": {
        "effect": "deployIfNotExists",
```

