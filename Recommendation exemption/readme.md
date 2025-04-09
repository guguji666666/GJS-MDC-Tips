# Create exemption in recommendation

## 1. Policy-based recommendation

## 2. Non policy-based recommendation
### 1. [List standards](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/security-standards/list?view=rest-defenderforcloud-2024-08-01&tabs=HTTP)

```
GET https://management.azure.com/{scope}/providers/Microsoft.Security/securityStandards?api-version=2024-08-01
```
Scope cound be `subscription` or `Security data connector such as AWS environmnet connected to Azure subscription`

In API response, search for the `assessmentkey id` and note the `id` it belongs to <br>

For example
```json
        {
            "properties": {
                "displayName": "Custom AWS standard 222",
                "description": "",
                "standardType": "Custom",
                "assessments": [
                    {
                        "assessmentKey": "a022693c-cfae-43a5-86d7-9bec995fdd9c"
                    },
                    {
                        "assessmentKey": "04e4dc63-ee46-405b-a02a-2a8395fe233d"
                    },
                    {
                        "assessmentKey": "9694d4ef-f21a-40b7-b535-618ac5c5d21e"
                    },
                    {
                        "assessmentKey": "036bb56b-c442-4352-bb4c-5bd0353ad314"
                    }
                ],
                "cloudProviders": [
                    "AWS"
                ]
            },
            "id": "/subscriptions/<sub id>/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/securityStandards/3d79fcde-7862-4ef9-a0d6-98c57cb9f879",
            "name": "3d79fcde-7862-4ef9-a0d6-98c57cb9f879",
            "type": "Microsoft.Security/securityStandards"
        }
```
* id : the standard id
* assessmentkey: we can find it in recommendation > query
