# Use ARG - Resource Graph Explorer

![image](https://user-images.githubusercontent.com/96930989/210159757-b875ba41-6946-4ee7-a604-92183cf9f58b.png)



# ARG list all subscriptions under your tenant

```kusto
resourcecontainers
| where type == "microsoft.resources/subscriptions"
| project name, id
```

# ARG list all subscriptions under specified management group

```kusto
resourcecontainers
| where type == 'microsoft.resources/subscriptions'
| mv-expand managementGroupParent = properties.managementGroupAncestorsChain
| where managementGroupParent.name =~ '<name of management group>'
| project name, id
| sort by name asc
```

# ARG list secure score of all subscriptions

```kusto
SecurityResources 
| where type == 'microsoft.security/securescores' 
| extend current = properties.score.current, max = todouble(properties.score.max)
| project subscriptionId, current, max, percentage = ((current / max)*100)
```
  
# ARG check relevant initiatives in subscription(basic)

```kusto
securityresources
| where type == "microsoft.security/assessments"
| where id contains "<RESOURCE-NAME>"
| where subscriptionId == "<SUBSCRIPTION-ID>"
| where name == "<ASSESSMENT-KEY>"
| extend initiatives = properties.statusPerInitiative
| mv-expand initiatives
| extend initiativeName = initiatives.policyInitiativeName
| extend statusInMdc = initiatives.assessmentStatus.code
| project initiativeName, statusInMdc
```

# ARG check relevant initiatives in subscription(advanced)

```kusto
securityresources
| where type == "microsoft.security/assessments"
| where id contains "<resource id>"
| where subscriptionId == "<subscription id>"
| extend initiatives = properties.statusPerInitiative
| extend  RecommendationName = properties.displayName
| extend  ResourceName = split(id, "/")[8]
| mv-expand initiatives
| extend initiativeName = initiatives.policyInitiativeName
| extend statusInMdc = initiatives.assessmentStatus.code
| where statusInMdc == "Unhealthy"
| project initiativeName, statusInMdc, RecommendationName, ResourceName
```
  
#  ARG compare results between MDC and Azure Policy

```kusto
securityresources
| where type == "microsoft.security/assessments"
| where subscriptionId == "<ADD-HERE-SUBSCRIPTION-ID>"
| where name == "ADD-HERE-ASSESSMENT-KEY" 
| where id contains "<ADD-HERE-RESOURCE-NAME>"
| extend resourceName = tostring(split(tolower(extract("(.*)/providers/Microsoft.Security",1, id)),"/")[-1])
| extend statusInMdc = properties.status.code
| project resourceName, statusInMdc
| join
(
policyresources
| where type == "microsoft.policyinsights/policystates"
| where subscriptionId == "<ADD-HERE-SUBSCRIPTION-ID>"
| where * contains "ADD-HERE-POLICY-DEFINITION-ID"
| where id contains "<ADD-HERE-RESOURCE-NAME>"
| extend resourceName = tostring(split(tolower(extract("(.*)/providers/microsoft.policyinsights",1, id)),"/")[-1])
| extend statusInPolicy = properties.complianceState
| project resourceName, statusInPolicy
) on resourceName
| project resourceName, statusInMdc, statusInPolicy
```
