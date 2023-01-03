# Use ARG - Resource Graph Explorer

## Navigate to Azure portal, and search `Resource Graph Explorer` in the top bar
![image](https://user-images.githubusercontent.com/96930989/210159757-b875ba41-6946-4ee7-a604-92183cf9f58b.png)



## ARG list all subscriptions under your tenant

```kusto
resourcecontainers
| where type == "microsoft.resources/subscriptions"
| project name, id
```

## ARG list all subscriptions under specified management group

```kusto
resourcecontainers
| where type == 'microsoft.resources/subscriptions'
| mv-expand managementGroupParent = properties.managementGroupAncestorsChain
| where managementGroupParent.name =~ '<name of management group>'
| project name, id
| sort by name asc
```

## ARG list secure score of all subscriptions

```kusto
SecurityResources 
| where type == 'microsoft.security/securescores' 
| extend current = properties.score.current, max = todouble(properties.score.max)
| project subscriptionId, current, max, percentage = ((current / max)*100)
```
  
## ARG check relevant initiatives in subscription(basic)

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

## ARG check relevant initiatives in subscription(advanced)

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

## ARG check relevant initiatives assigned and exemption

```kusto
policyresources
| where type == "microsoft.policyinsights/policystates"
| where id contains "<RESOURCE-NAME>"
| where subscriptionId == "<SUBSCRIPTION-ID>"
| where properties.policyDefinitionId contains "<POLICY-DEFINITION-ID>"
| extend policySetDefinitionId = extract("policySetDefinitions/(.*)", 1, tostring(properties.policySetDefinitionId))
| extend status = properties.complianceState
| project policySetDefinitionId, status
| summarize statuses = make_set(status) by policySetDefinitionId
```

##  ARG compare results between MDC and Azure Policy

```kusto
securityresources
| where type == "microsoft.security/assessments"
| where subscriptionId == "<ADD-HERE-SUBSCRIPTION-ID>"
| where name == "<ADD-HERE-ASSESSMENT-KEY>" 
| where id contains "<ADD-HERE-RESOURCE-NAME>"
| extend resourceName = tostring(split(tolower(extract("(.*)/providers/Microsoft.Security",1, id)),"/")[-1])
| extend statusInMdc = properties.status.code
| project resourceName, statusInMdc
| join
(
policyresources
| where type == "microsoft.policyinsights/policystates"
| where subscriptionId == "`<ADD-HERE-SUBSCRIPTION-ID>"
| where * contains "<ADD-HERE-POLICY-DEFINITION-ID>"
| where id contains "<ADD-HERE-RESOURCE-NAME>"
| extend resourceName = tostring(split(tolower(extract("(.*)/providers/microsoft.policyinsights",1, id)),"/")[-1])
| extend statusInPolicy = properties.complianceState
| project resourceName, statusInPolicy
) on resourceName
| project resourceName, statusInMdc, statusInPolicy
```

### Find `ASSESSMENT-KEY` from recommendation

![image](https://user-images.githubusercontent.com/96930989/210167530-9396be11-ec9e-4119-afae-61161651ecc8.png)

![image](https://user-images.githubusercontent.com/96930989/210167541-5f485618-02c5-4328-81bc-53312a216b31.png)

### Find `POLICY-DEFINITION-ID` from recommendation

![image](https://user-images.githubusercontent.com/96930989/210167491-c93bf905-d9f9-41b4-a95c-ab2eb77f9e54.png)

### Copy the last part of the `Definition ID`
![image](https://user-images.githubusercontent.com/96930989/210167501-18c46574-1d14-4f58-8a60-5d24ebedd3bc.png)
