# Manage defender for cloud price tier

## API reference
[Microsoft Defender for Cloud API](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/)
[Pricings](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/pricings)
[Settings](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/settings)


## Enable CSPM extensions
[Update price tier API](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/pricings/update?tabs=HTTP#code-try-0)

### Binding
```
Put
```
### url
```
https://management.azure.com/subscriptions/<your subscription id>/providers/Microsoft.Security/pricings/CloudPosture?api-version=2023-01-01
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
