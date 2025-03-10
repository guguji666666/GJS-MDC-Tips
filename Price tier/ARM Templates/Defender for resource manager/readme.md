# Defender for resource manager

## Difference between legacy and new plan
![image](https://github.com/user-attachments/assets/b2d33489-4409-4d19-ba13-c3f62e544aed)

## ARM template (Legacy Per-API-call mode)
```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Security/pricings",
            "apiVersion": "2023-01-01",
            "name": "Arm",
            "properties": {
                "pricingTier": "Standard",
                "subPlan": "PerApiCall"
            }
        }
    ]
}
```

## ARM template (New Per-subscription plan)
```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Security/pricings",
            "apiVersion": "2023-01-01",
            "name": "Arm",
            "properties": {
                "pricingTier": "Standard",
                "subPlan": "PerSubscription"
            }
        }
    ]
}
```
