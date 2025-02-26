# Defender for App service
## ARM template
```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Security/pricings",
            "apiVersion": "2023-01-01",
            "name": "AppServices",
            "properties": {
                "pricingTier": "Standard"
            }
        }
    ]
}
```
