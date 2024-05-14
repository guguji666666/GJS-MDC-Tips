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
    policyDefinitionReferenceId = tostring(properties.policyDefinitionReferenceId),
    //stateWeight = iff(complianceState == 'NonCompliant', int(300), iff(complianceState == 'Compliant', int(200), iff(complianceState == 'Conflict', int(100), iff(complianceState == 'Exempt', int(50), int(0)))))
| project resourceId, policyAssignmentId, policyAssignmentName, policyDefinitionId, policyDefinitionReferenceId
```


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
