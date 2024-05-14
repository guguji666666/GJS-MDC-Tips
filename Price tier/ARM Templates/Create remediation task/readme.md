# Deploy remediation task using ARM template

## ARG query get policy assignment ID and policy definition ID
```kql
PolicyResources
| where type =~ 'Microsoft.PolicyInsights/PolicyStates'
| extend complianceState = tostring(properties.complianceState)
| extend
    resourceId = tostring(properties.resourceId),
    policyAssignmentId = tostring(properties.policyAssignmentId),
    policyAssignmentScope = tostring(properties.policyAssignmentScope),
    policyAssignmentName = tostring(properties.policyAssignmentName),
    policyDefinitionId = tostring(properties.policyDefinitionId),
    policyDefinitionReferenceId = tostring(properties.policyDefinitionReferenceId)
| project resourceId, policyAssignmentId, policyAssignmentName, policyDefinitionId, policyDefinitionReferenceId
```

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/785f8283-bbcf-445d-9078-86bba4b7b261)

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/a1f971a1-053b-4e69-83bd-0d65e02a3e30)


You can also check Policy assignment ID and policy definition ID here (below is the sample) <br>

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/5768e328-7cc8-413b-a424-b5468ef5e236)

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/8d2998d9-4392-4e6a-bb0c-7a07deb2607b)

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/7c2be245-416c-4694-a6bf-f82d6f4f8896)

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/c1948620-82ed-4ef6-8d6a-95a7552401d2)


## ARM template for deployment

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "remediationName": {
            "type": "String",
            "metadata": {
                "description": "The name of the remediation."
            }
        },
        "subscriptionId": {
            "type": "String",
            "metadata": {
                "description": "The subscription ID where the remediation is deployed."
            }
        },
        "policyAssignmentId": {
            "type": "String",
            "metadata": {
                "description": "The ID of the policy assignment to be remediated."
            }
        },
        "policyDefinitionReferenceId": {
            "defaultValue": "",
            "type": "String",
            "metadata": {
                "description": "The reference ID of the policy definition within an initiative. Leave empty if not applicable."
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.PolicyInsights/remediations",
            "apiVersion": "2021-10-01",
            "name": "[parameters('remediationName')]",
            "properties": {
                "failureThreshold": {
                    "percentage": 0.5
                },
                "filters": {
                    "locations": [
                        "<enter location here>"
                    ]
                },
                "parallelDeployments": 10,
                "policyAssignmentId": "[parameters('policyAssignmentId')]",
                "policyDefinitionReferenceId": "[parameters('policyDefinitionReferenceId')]",
                "resourceCount": 100,
                "resourceDiscoveryMode": "ExistingNonCompliant"
            }
        }
    ]
}
