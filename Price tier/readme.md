# Manage defender for cloud price tier

## API reference
* [Microsoft Defender for Cloud API](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/)
* [Pricings](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/pricings)
* [Settings](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/settings)

# 1. Defender for cloud settings
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
```json
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
```json
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

# 2. Defender for cloud pricings
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
```json
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

## [Get defender for cloud pricing](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/pricings/get?tabs=HTTP)

## Update defender for cloud pricing

### Binding
```
PUT
```

### URL
```
PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/{pricingName}?api-version=2023-01-01
```

### PricingName
* VirtualMachines
* SqlServers
* AppServices
* StorageAccounts
* SqlServerVirtualMachines
* KeyVaults
* ArmOpenSourceRelationalDatabases
* CosmosDbs
* Containers
* CloudPosture
* Api

### Sample --- Enable defender for server plan P2

```
PUT
```

### URL
```
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/VirtualMachines?api-version=2023-01-01
```

### Request Body
```json
{
  "properties": {
    "extensions": [
      {
        "name": "MdeDesignatedSubscription",
        "isEnabled": "False"
      },
      {
        "name": "AgentlessVmScanning",
        "isEnabled": "True",
        "additionalExtensionProperties": {
          "ExclusionTags": "[]"
        }
      }
    ],
    "subPlan": "P2",
    "pricingTier": "Standard"
  }
}
```

### Sample response
```json
{
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/VirtualMachines",
  "name": "VirtualMachines",
  "type": "Microsoft.Security/pricings",
  "properties": {
    "extensions": [
      {
        "name": "MdeDesignatedSubscription",
        "isEnabled": "False"
      },
      {
        "name": "AgentlessVmScanning",
        "isEnabled": "True",
        "additionalExtensionProperties": {
          "ExclusionTags": "[]"
        },
        "operationStatus": {
          "code": "Succeeded",
          "message": "Successfully enabled extension"
        }
      }
    ],
    "enablementTime": "2023-09-24T03:56:50.3584879Z",
    "subPlan": "P2",
    "pricingTier": "Standard",
    "freeTrialRemainingTime": "PT0S"
  }
}
```

### Sample --- Enable defender for storage V2 (per-storage plan) with Malware Scanning functions

```
PUT
```

### URL
```
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Security/pricings/StorageAccounts?api-version=2023-01-01
```

### Request body
```json
{
  "properties": {
    "extensions": [
      {
        "name": "OnUploadMalwareScanning",
        "isEnabled": "True",
        "additionalExtensionProperties": {
          "CapGBPerMonthPerStorageAccount": "5000"
        }
      },
      {
        "name": "SensitiveDataDiscovery",
        "isEnabled": "True"
      }
    ],
    "subPlan": "DefenderForStorageV2",
    "pricingTier": "Standard"
  }
}
```

### Sample response
```json
```

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/e388e360-e486-47d6-b4eb-adb5e4d9aa6c)


# 3. Defender for cloud CSPM

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
