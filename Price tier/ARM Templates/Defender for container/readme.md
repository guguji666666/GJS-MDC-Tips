
# Enable defender for container plan using ARM

## Enable the plan
```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Security/pricings",
            "apiVersion": "2023-01-01",
            "name": "Containers",
            "properties": {
                "pricingTier": "Standard",
                "extensions": [
                    {
                        "name": "ContainerRegistriesVulnerabilityAssessments",
                        "isEnabled": "True"
                    },
                    {
                        "name": "AgentlessDiscoveryForKubernetes",
                        "isEnabled": "True"
                    },
                    {
                        "name": "AgentlessVmScanning",
                        "isEnabled": "False"
                    },
                    {
                        "name": "ContainerSensor",
                        "isEnabled": "True"
                    }
                ]
            }
        }
    ]
}
```


```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Security/pricings",
            "apiVersion": "2023-01-01",
            "name": "Containers",
            "properties": {
                "pricingTier": "Standard",
                "extensions": [
                    {
                        "name": "ContainerRegistriesVulnerabilityAssessments",
                        "isEnabled": "True"
                    },
                    {
                        "name": "AgentlessDiscoveryForKubernetes",
                        "isEnabled": "True"
                    }
                ]
            }
        }
    ]
}
```


## Disable the plan
```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Security/pricings",
            "apiVersion": "2023-01-01",
            "name": "Containers",
            "properties": {
                "pricingTier": "Free"
 
            }
        }
    ]
}
```
