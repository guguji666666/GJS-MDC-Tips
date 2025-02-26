# Defender for SQL
## ARM template
```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Security/pricings",
            "apiVersion": "2023-01-01",
            "name": "SqlServerVirtualMachines",
            "properties": {
                "pricingTier": "Standard"
            }
        },
        {
            "type": "Microsoft.Security/pricings",
            "apiVersion": "2023-01-01",
            "name": "OpenSourceRelationalDatabases",
            "properties": {
                "pricingTier": "Standard"
            }
        },
        {
            "type": "Microsoft.Security/pricings",
            "apiVersion": "2023-01-01",
            "name": "CosmosDbs",
            "properties": {
                "pricingTier": "Standard"
            }
        },
        {
            "type": "Microsoft.Security/pricings",
            "apiVersion": "2023-01-01",
            "name": "SqlServers",
            "properties": {
                "pricingTier": "Standard"
            }
        }
    ]
}
```

![image](https://github.com/user-attachments/assets/e462fe0b-762b-4d7c-ab91-c830ac8e9d33)

![image](https://github.com/user-attachments/assets/7e3e7c60-f0fa-4768-affe-beb63588a914)
