# Defender for storage
## ARM template
```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Security/pricings",
            "apiVersion": "2023-01-01",
            "name": "StorageAccounts",
            "properties": {
                "extensions": [
                    {
                        "name": "OnUploadMalwareScanning",
                        "isEnabled": "True",
                        "additionalExtensionProperties": {
                            "CapGBPerMonthPerStorageAccount": "8000" //You can customize the monthly cap here
                        }
                    },
                    {
                        "name": "SensitiveDataDiscovery",
                        "isEnabled": "True"
                    }
                ],
                "pricingTier": "Standard",
                "subPlan": "DefenderForStorageV2"
            }
        }
    ]
}
```
