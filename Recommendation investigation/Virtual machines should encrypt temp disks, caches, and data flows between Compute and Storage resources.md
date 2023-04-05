### Virtual machines should encrypt temp disks, caches, and data flows between Compute and Storage resources

#### Information about this recommendation

Policy name
```
Virtual machines should encrypt temp disks, caches, and data flows between Compute and Storage resources
```

Policy Definition ID
```
/providers/Microsoft.Authorization/policyDefinitions/0961003e-5a0a-4549-abde-af6a37f2724d
```

Policy Effect
```
Audit If Not Exists, Disabled
```

Assessment key
```
d57a4221-a804-52ca-3dea-768284f06bb7
```


#### ARG query to compare the results between Azure policy and defender for cloud

Navigate to Azure portal, and search `Resource Graph Explorer` on the top

![image](https://user-images.githubusercontent.com/96930989/210159757-b875ba41-6946-4ee7-a604-92183cf9f58b.png)

```kusto
securityresources
| where type == "microsoft.security/assessments"
| where subscriptionId == "<ADD-HERE-SUBSCRIPTION-ID>"
| where name == "d57a4221-a804-52ca-3dea-768284f06bb7" 
| where id contains "<ADD-HERE-RESOURCE-NAME>"
| extend resourceName = tostring(split(tolower(extract("(.*)/providers/Microsoft.Security",1, id)),"/")[-1])
| extend statusInMdc = properties.status.code
| project resourceName, statusInMdc
| join
(
policyresources
| where type == "microsoft.policyinsights/policystates"
| where subscriptionId == "<ADD-HERE-SUBSCRIPTION-ID>"
| where * contains "0961003e-5a0a-4549-abde-af6a37f2724d"
| where id contains "<ADD-HERE-RESOURCE-NAME>"
| extend resourceName = tostring(split(tolower(extract("(.*)/providers/microsoft.policyinsights",1, id)),"/")[-1])
| extend statusInPolicy = properties.complianceState
| project resourceName, statusInPolicy
) on resourceName
| project resourceName, statusInMdc, statusInPolicy
```

