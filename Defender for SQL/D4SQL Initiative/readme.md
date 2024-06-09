# D4SQL AMA auto-provisioning

## Before we start

### [List defender for cloud price tier](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/pricings/list?view=rest-defenderforcloud-2024-01-01&tabs=HTTP)
#### Sample output (i have enabled AMA auto-provisioning in D4SQL)
Binding
```
GET
```
URL
```
https://management.azure.com/{scopeId}/providers/Microsoft.Security/pricings?api-version=2024-01-01
```
Powershell comamnds to get user token used in postmam
```powershell
Set-ExecutionPolicy RemoteSigned

Connect-AzAccount -TenantId <tenant id>

Set-AzContext -Subscription <subscription id>

$accessToken = (Get-AzAccessToken).Token

$accessToken | Set-Clipboard
```

List pricings
```json
{
    "value": [
        {
            "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/VirtualMachines",
            "name": "VirtualMachines",
            "type": "Microsoft.Security/pricings",
            "properties": {
                "resourcesCoverageStatus": "FullyCovered",
                "extensions": [
                    {
                        "name": "MdeDesignatedSubscription",
                        "isEnabled": "False"
                    }
                ],
                "subPlan": "P1",
                "pricingTier": "Standard",
                "freeTrialRemainingTime": "PT0S"
            }
        },
        {
            "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/SqlServers",
            "name": "SqlServers",
            "type": "Microsoft.Security/pricings",
            "properties": {
                "resourcesCoverageStatus": "FullyCovered",
                "enablementTime": "2024-06-07T00:08:32.7642676Z",
                "pricingTier": "Standard",
                "freeTrialRemainingTime": "P27DT21H52M"
            }
        },
        {
            "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/AppServices",
            "name": "AppServices",
            "type": "Microsoft.Security/pricings",
            "properties": {
                "resourcesCoverageStatus": "NotCovered",
                "pricingTier": "Free",
                "freeTrialRemainingTime": "P30D"
            }
        },
        {
            "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/StorageAccounts",
            "name": "StorageAccounts",
            "type": "Microsoft.Security/pricings",
            "properties": {
                "resourcesCoverageStatus": "NotCovered",
                "pricingTier": "Free",
                "freeTrialRemainingTime": "P30D"
            }
        },
        {
            "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/SqlServerVirtualMachines",
            "name": "SqlServerVirtualMachines",
            "type": "Microsoft.Security/pricings",
            "properties": {
                "resourcesCoverageStatus": "FullyCovered",
                "enablementTime": "2024-06-07T00:08:36.7684333Z",
                "pricingTier": "Standard",
                "freeTrialRemainingTime": "P27DT21H52M"
            }
        },
        {
            "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/KubernetesService",
            "name": "KubernetesService",
            "type": "Microsoft.Security/pricings",
            "properties": {
                "resourcesCoverageStatus": "NotCovered",
                "pricingTier": "Free",
                "freeTrialRemainingTime": "P30D",
                "deprecated": true,
                "replacedBy": [
                    "Containers"
                ]
            }
        },
        {
            "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/ContainerRegistry",
            "name": "ContainerRegistry",
            "type": "Microsoft.Security/pricings",
            "properties": {
                "resourcesCoverageStatus": "NotCovered",
                "pricingTier": "Free",
                "freeTrialRemainingTime": "P30D",
                "deprecated": true,
                "replacedBy": [
                    "Containers"
                ]
            }
        },
        {
            "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/KeyVaults",
            "name": "KeyVaults",
            "type": "Microsoft.Security/pricings",
            "properties": {
                "resourcesCoverageStatus": "NotCovered",
                "pricingTier": "Free",
                "freeTrialRemainingTime": "P30D"
            }
        },
        {
            "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/Dns",
            "name": "Dns",
            "type": "Microsoft.Security/pricings",
            "properties": {
                "resourcesCoverageStatus": "NotCovered",
                "pricingTier": "Free",
                "freeTrialRemainingTime": "P30D",
                "deprecated": true,
                "replacedBy": [
                    "VirtualMachines"
                ]
            }
        },
        {
            "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/Arm",
            "name": "Arm",
            "type": "Microsoft.Security/pricings",
            "properties": {
                "resourcesCoverageStatus": "NotCovered",
                "pricingTier": "Free",
                "freeTrialRemainingTime": "P30D"
            }
        },
        {
            "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/OpenSourceRelationalDatabases",
            "name": "OpenSourceRelationalDatabases",
            "type": "Microsoft.Security/pricings",
            "properties": {
                "resourcesCoverageStatus": "FullyCovered",
                "enablementTime": "2024-06-07T00:08:39.2283335Z",
                "pricingTier": "Standard",
                "freeTrialRemainingTime": "P27DT21H52M"
            }
        },
        {
            "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/CosmosDbs",
            "name": "CosmosDbs",
            "type": "Microsoft.Security/pricings",
            "properties": {
                "resourcesCoverageStatus": "FullyCovered",
                "enablementTime": "2024-06-07T00:08:43.5426477Z",
                "pricingTier": "Standard",
                "freeTrialRemainingTime": "P27DT21H52M"
            }
        },
        {
            "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/Containers",
            "name": "Containers",
            "type": "Microsoft.Security/pricings",
            "properties": {
                "resourcesCoverageStatus": "NotCovered",
                "pricingTier": "Free",
                "freeTrialRemainingTime": "P30D"
            }
        },
        {
            "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/CloudPosture",
            "name": "CloudPosture",
            "type": "Microsoft.Security/pricings",
            "properties": {
                "resourcesCoverageStatus": "FullyCovered",
                "extensions": [
                    {
                        "name": "SensitiveDataDiscovery",
                        "isEnabled": "True"
                    },
                    {
                        "name": "ContainerRegistriesVulnerabilityAssessments",
                        "isEnabled": "True"
                    },
                    {
                        "name": "AgentlessDiscoveryForKubernetes",
                        "isEnabled": "False"
                    },
                    {
                        "name": "AgentlessVmScanning",
                        "isEnabled": "False"
                    },
                    {
                        "name": "EntraPermissionsManagement",
                        "isEnabled": "False"
                    }
                ],
                "enablementTime": "2023-03-29T03:32:24.3714493Z",
                "pricingTier": "Standard",
                "freeTrialRemainingTime": "PT0S"
            }
        },
        {
            "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/Api",
            "name": "Api",
            "type": "Microsoft.Security/pricings",
            "properties": {
                "resourcesCoverageStatus": "NotCovered",
                "pricingTier": "Free",
                "freeTrialRemainingTime": "P30D"
            }
        }
    ]
}
```


### [Get pricing defender for server](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/pricings/get?view=rest-defenderforcloud-2024-01-01&tabs=HTTP)

```
GET
```

```
https://management.azure.com/{scopeId}/providers/Microsoft.Security/pricings/{pricingName}?api-version=2024-01-01
```

```
https://management.azure.com/subscriptions/<sub id>/providers/Microsoft.Security/pricings/SqlServerVirtualMachines?api-version=2024-01-01
```
```json
{
    "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/SqlServerVirtualMachines",
    "name": "SqlServerVirtualMachines",
    "type": "Microsoft.Security/pricings",
    "properties": {
        "resourcesCoverageStatus": "FullyCovered",
        "enablementTime": "2024-06-07T00:08:36.7684333Z",
        "pricingTier": "Standard",
        "freeTrialRemainingTime": "P27DT21H47M"
    }
}
```

```
https://management.azure.com/subscriptions/<sub id>/providers/Microsoft.Security/pricings/VirtualMachines?api-version=2024-01-01
```
```json
{
    "id": "/subscriptions/77d5e475-228a-44bf-94a9-c3d511778610/providers/Microsoft.Security/pricings/VirtualMachines",
    "name": "VirtualMachines",
    "type": "Microsoft.Security/pricings",
    "properties": {
        "resourcesCoverageStatus": "FullyCovered",
        "extensions": [
            {
                "name": "MdeDesignatedSubscription",
                "isEnabled": "False"
            }
        ],
        "subPlan": "P1",
        "pricingTier": "Standard",
        "freeTrialRemainingTime": "PT0S"
    }
}
```

### Conclusion
According to the response from REST API, it seems that the AMA auto-provisioning for D4SQL couldn't be found under any price tier or extension, which means we can't enable it using the normal ARM template we used to enable other defender for cloud price tier.


# Check D4SQL relating intiative and behavior in panel
## 1. Enable `Azure Monitoring Agent for SQL server on machines` in panel
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/ea4a6d04-ceb2-4b94-bf9d-6f0bd0060539)

### Activity logs
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/ec092b04-7f13-47f2-ab5f-cd45827a1aa8)

### Remediation tasks automatically created
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/8293c4d4-3a22-4b8e-8fce-c88477882a74)

#### Conclusion
When enabling `Azure Monitoring Agent for SQL server on machines` in panel, relating polices are assgined automatically and so do remediation tasks


## 2. Assign initiative "Configure SQL VMs and Arc-enabled SQL Servers to install Microsoft Defender for SQL and AMA with a user-defined LA workspace"

### Initiative info
```
"/providers/Microsoft.Authorization/policySetDefinitions/de01d381-bae9-4670-8870-786f89f49e26"
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/97c458a9-2a84-47f4-9075-a4c0ba43b299)


#### Assign the intiative to subscription level manually
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/6d88bacf-d0a5-4716-9bac-57c2faa1a726)

#### `Azure Monitoring Agent for SQL server on machines` is then be enabled and configured
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/ea9e6d9c-e35c-48cc-96f8-f46fc3789c39)


#### Conclusion
When assigning initiative "Configure SQL VMs and Arc-enabled SQL Servers to install Microsoft Defender for SQL and AMA with a user-defined LA workspace" to the subscription level, although the poclices inside doesn't reach directly to price tier or extension below it, the function `Azure Monitoring Agent for SQL server on machines` will still be enabled, which looks like a backend behavior
