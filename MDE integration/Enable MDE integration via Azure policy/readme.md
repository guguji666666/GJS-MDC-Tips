# Enable MDE integration via Azure policy

## 1. Navigate to Policy > Definitions > + Policy definition
## 2. Select the "Definition location"

If you want to assign the policy to multiple subscriptions or management group level, select the management group in the scope 

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/7e908e1b-c192-4836-bee2-093b5054ccdf)

Define the policy rule 

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/3e916a03-8e3a-44ab-bcf6-bee9dd860631)

Copy the definition below <br>
```json
{
  "mode": "All",
  "policyRule": {
    "if": {
      "allOf": [
        {
          "field": "type",
          "equals": "Microsoft.Resources/subscriptions"
        }
      ]
    },
    "then": {
      "effect": "deployIfNotExists",
      "details": {
        "type": "Microsoft.Security/settings",
        "name": "WDATP",
        "deploymentScope": "subscription",
        "existenceScope": "subscription",
        "roleDefinitionIds": [
          "/providers/Microsoft.Authorization/roleDefinitions/fb1c8493-542b-48eb-b624-b4c8fea62acd"
        ],
        "existenceCondition": {
          "field": "Microsoft.Security/settings/DataExportSetting.enabled",
          "equals": true
        },
        "deployment": {
          "location": "westeurope",
          "properties": {
            "mode": "incremental",
            "parameters": {},
            "template": {
              "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
              "contentVersion": "1.0.0.0",
              "parameters": {},
              "variables": {},
              "resources": [
                {
                  "type": "Microsoft.Security/settings",
                  "apiVersion": "2019-01-01",
                  "name": "WDATP",
                  "kind": "DataExportSettings",
                  "properties": {
                    "enabled": true
                  }
                }
              ],
              "outputs": {}
            }
          }
        }
      }
    }
  },
  "parameters": {}
}
```
Save the configuration

## 3. Assign the policy

Define the assignment scope and assignment name for easy recognition

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/6b951f65-55a3-485d-9866-91dc161955ef)

Under remediation, check the box of `Create a remediation task` so that the existing subscriptions will enable MDE integration as well

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/55ed3349-609f-46e0-bc08-280040843b63)

Review the configuration and create the assignment

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/4d1e21a5-6d60-41b7-a251-4c3d88138513)

Then we just wait until the remediation task finishes, the time it takes depends on the resources in your environment.

We can check the status of remediation task here

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/9493283b-ff50-4846-a67b-50c6da8e4519)
