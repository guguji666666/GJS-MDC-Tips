# Enable defender for cloud plan using API

## Method
```
PUT
```

## 1.Defender for server
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

