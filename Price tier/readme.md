# Manage defender for cloud price tier

## API reference
* [Microsoft Defender for Cloud API](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/)
* [Pricings](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/pricings)
* [Settings](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/settings)


## List defender for cloud settings

### Binding
```
Get
```

### URL
```
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Security/settings?api-version=2021-06-01
```

Sample output
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


## Enable CSPM and all its extensions
[Update price tier API](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/pricings/update?tabs=HTTP#code-try-0)

### Binding
```
Put
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
