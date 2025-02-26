# Defender for server

## ARM template (will encounter error)
```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Security/pricings",
            "apiVersion": "2023-01-01",
            "name": "VirtualMachines",
            "properties": {
                "pricingTier": "Standard",
                "subPlan": "P2",
                "extensions": [
                    {
                        "name": "MdeDesignatedSubscription",
                        "isEnabled": "False"
                    },
                    {
                        "name": "AgentlessVmScanning",
                        "isEnabled": "True",
                        "additionalExtensionProperties": {
                            "ExclusionTags": "[json('[]')]"
                        }
                    },
                    {
                        "name": "FileIntegrityMonitoring",
                        "isEnabled": "False"
                    }
                ]
            }
        }
    ]
}
```

## API (recommended)
### Method
```
PUT
```

### URL
```
https://management.azure.com/subscriptions/<subscription id>/providers/Microsoft.Security/pricings/VirtualMachines?api-version=2023-01-01
```

### Request body
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
            },
            {
                "name": "FileIntegrityMonitoring",
                "isEnabled": "False"
            }
        ],
        "subPlan": "P2",
        "pricingTier": "Standard"
    }
}
```
