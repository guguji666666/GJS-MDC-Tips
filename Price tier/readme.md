# Manage defender for cloud price tier

## API reference
* [Microsoft Defender for Cloud API](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/)
* [Pricings](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/pricings)
* [Settings](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/settings)

## List defender for cloud settings

### Binding
```
GET
```

### URL
```
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Security/settings?api-version=2021-06-01
```

### Sample response
```
{
  "value": [
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/settings/MCAS",
      "name": "MCAS",
      "type": "Microsoft.Security/settings",
      "kind": "DataExportSettings",
      "properties": {
        "enabled": true
      }
    },
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/settings/WDATP",
      "name": "WDATP",
      "type": "Microsoft.Security/settings",
      "kind": "DataExportSettings",
      "properties": {
        "enabled": true
      }
    },
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/settings/Sentinel",
      "name": "Sentinel",
      "type": "Microsoft.Security/settings",
      "kind": "AlertSyncSettings",
      "properties": {
        "enabled": true
      }
    }
  ]
}
```

## Update defender for cloud settings (MDE integration)

### Binding
```
PUT
```

### URL
```
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Security/settings/WDATP?api-version=2021-06-01
```

### Request body
```
{
  "kind": "DataExportSettings",
  "properties": {
    "enabled": true
  }
}
```

### Sample response
```
{
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/settings/WDATP",
  "name": "WDATP",
  "type": "Microsoft.Security/settings",
  "kind": "DataExportSettings",
  "properties": {
    "enabled": true
  }
}
```

## List defender for cloud pricings

### Binding
```
GET
```

### URL
```
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings?api-version=2023-01-01
```

### Sample response
```
{
  "value": [
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/VirtualMachines",
      "name": "VirtualMachines",
      "type": "Microsoft.Security/pricings",
      "properties": {
        "pricingTier": "Free",
        "freeTrialRemainingTime": "PT0S"
      }
    },
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/SqlServers",
      "name": "SqlServers",
      "type": "Microsoft.Security/pricings",
      "properties": {
        "pricingTier": "Free",
        "freeTrialRemainingTime": "PT0S"
      }
    },
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/AppServices",
      "name": "AppServices",
      "type": "Microsoft.Security/pricings",
      "properties": {
        "pricingTier": "Free",
        "freeTrialRemainingTime": "PT0S"
      }
    },
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/StorageAccounts",
      "name": "StorageAccounts",
      "type": "Microsoft.Security/pricings",
      "properties": {
        "pricingTier": "Free",
        "freeTrialRemainingTime": "PT0S"
      }
    },
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/SqlServerVirtualMachines",
      "name": "SqlServerVirtualMachines",
      "type": "Microsoft.Security/pricings",
      "properties": {
        "pricingTier": "Free",
        "freeTrialRemainingTime": "PT0S"
      }
    },
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/KubernetesService",
      "name": "KubernetesService",
      "type": "Microsoft.Security/pricings",
      "properties": {
        "pricingTier": "Free",
        "freeTrialRemainingTime": "PT0S",
        "deprecated": true,
        "replacedBy": [
          "Containers"
        ]
      }
    },
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/ContainerRegistry",
      "name": "ContainerRegistry",
      "type": "Microsoft.Security/pricings",
      "properties": {
        "pricingTier": "Free",
        "freeTrialRemainingTime": "PT0S",
        "deprecated": true,
        "replacedBy": [
          "Containers"
        ]
      }
    },
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/KeyVaults",
      "name": "KeyVaults",
      "type": "Microsoft.Security/pricings",
      "properties": {
        "pricingTier": "Free",
        "freeTrialRemainingTime": "PT0S"
      }
    },
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/Dns",
      "name": "Dns",
      "type": "Microsoft.Security/pricings",
      "properties": {
        "pricingTier": "Standard",
        "freeTrialRemainingTime": "PT0S",
        "deprecated": true,
        "replacedBy": [
          "VirtualMachines"
        ]
      }
    },
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/Arm",
      "name": "Arm",
      "type": "Microsoft.Security/pricings",
      "properties": {
        "subPlan": "PerApiCall",
        "pricingTier": "Standard",
        "freeTrialRemainingTime": "PT0S"
      }
    },
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/OpenSourceRelationalDatabases",
      "name": "OpenSourceRelationalDatabases",
      "type": "Microsoft.Security/pricings",
      "properties": {
        "pricingTier": "Free",
        "freeTrialRemainingTime": "PT0S"
      }
    },
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/CosmosDbs",
      "name": "CosmosDbs",
      "type": "Microsoft.Security/pricings",
      "properties": {
        "pricingTier": "Free",
        "freeTrialRemainingTime": "PT0S"
      }
    },
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/Containers",
      "name": "Containers",
      "type": "Microsoft.Security/pricings",
      "properties": {
        "pricingTier": "Free",
        "freeTrialRemainingTime": "PT0S"
      }
    },
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/CloudPosture",
      "name": "CloudPosture",
      "type": "Microsoft.Security/pricings",
      "properties": {
        "pricingTier": "Free",
        "freeTrialRemainingTime": "PT0S"
      }
    },
    {
      "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/Api",
      "name": "Api",
      "type": "Microsoft.Security/pricings",
      "properties": {
        "pricingTier": "Free",
        "freeTrialRemainingTime": "PT0S"
      }
    }
  ]
}
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/3434d23b-30ab-448f-8979-d9e46844870b)


## Enable CSPM and all its extensions

### Binding
```
PUT
```
### URL
```
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/CloudPosture?api-version=2023-01-01
```

### Body
```json
{
  "properties": {
          "pricingTier": "Standard",
          "extensions": [
            {
              "name": "AgentlessVmScanning",
              "isEnabled": "True"
            },
            {
              "name": "AgentlessDiscoveryForKubernetes",
              "isEnabled": "True"
            },
            {
              "name": "SensitiveDataDiscovery",
              "isEnabled": "True"
            },
            {
              "name": "ContainerRegistriesVulnerabilityAssessments",
              "isEnabled": "True"
            }
        ]
       }
  }
```
