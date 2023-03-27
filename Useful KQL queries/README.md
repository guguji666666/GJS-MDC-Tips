# Use ARG - Resource Graph Explorer

### Navigate to Azure portal, and search `Resource Graph Explorer` on the top
![image](https://user-images.githubusercontent.com/96930989/210159757-b875ba41-6946-4ee7-a604-92183cf9f58b.png)

## 1. ARG list all subscriptions under your tenant

```kusto
resourcecontainers
| where type == "microsoft.resources/subscriptions"
| project name, id
```

## 2. ARG list all subscriptions under specified management group

```kusto
resourcecontainers
| where type == 'microsoft.resources/subscriptions'
| mv-expand managementGroupParent = properties.managementGroupAncestorsChain
| where managementGroupParent.name =~ '<name of management group>'
| project name, id
| sort by name asc
```

## 3. ARG list current secure score of all subscriptions

```kusto
SecurityResources 
| where type == 'microsoft.security/securescores' 
| extend current = properties.score.current, max = todouble(properties.score.max)
| project subscriptionId, current, max, percentage = ((current / max)*100)
```
  
## 4. ARG check relevant initiatives in subscription (basic)

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

## 5. ARG check relevant initiatives in subscription (advanced)

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

## 6. ARG check relevant initiatives assigned and exemption

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

##  7. ARG compare results between Defender for cloud and Azure Policy

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

##  8. ARG list all unpatched VM along with OS information
```
MDC Recommendation: System updates should be installed on your machines
```
### ARG Table 1 : Unpatched VM with resource id (filter with `Unhealty` and `NotApplicable` state)
```kusto
securityresources
        | where type == "microsoft.security/assessments"
        | extend source = trim(' ', tolower(tostring(properties.resourceDetails.Source)))
                                          | extend resourceId = trim(' ', tolower(tostring(case(
                                                                                    source =~ "azure", properties.resourceDetails.Id,
                                                                                    source =~ "aws" and isnotempty(tostring(properties.resourceDetails.ConnectorId)), properties.resourceDetails.Id,
                                                                                    source =~ "gcp" and isnotempty(tostring(properties.resourceDetails.ConnectorId)), properties.resourceDetails.Id,
                                                                                    source =~ 'aws', properties.resourceDetails.AzureResourceId,
                                                                                    source =~ 'gcp', properties.resourceDetails.AzureResourceId,
                                                                                    extract('^(.+)/providers/Microsoft.Security/assessments/.+$',1,id)
                                                                                    ))))
        | extend status = trim(" ", tostring(properties.status.code))
        | extend cause = trim(" ", tostring(properties.status.cause))
        | extend assessmentKey = tostring(name)
        | where assessmentKey == "4ab6e3c5-74dd-8b35-9ab9-f61b30875b27"
        | where status == "Unhealthy" or status == "NotApplicable" 
        | project subscriptionId, id=resourceId, status, assessmentKey, cause 
```
![image](https://user-images.githubusercontent.com/96930989/210491477-5eff6f65-1010-4e22-8764-05f50686ccc4.png)

Then download the csv file, let's name it `table1`

### ARG Table 2 : Detailed information of all Azure VMs

```kusto
Resources
| where type == "microsoft.compute/virtualmachines"
| extend vmName = properties.osProfile.computerName
| extend osOffer = properties.storageProfile.imageReference.offer
| extend osSku = properties.storageProfile.imageReference.sku
| extend osName = properties.extended.instanceView.osName
| extend osVersion = properties.extended.instanceView.osVersion
| project id, resourceGroup, location, osOffer, osSku
```
![image](https://user-images.githubusercontent.com/96930989/211176909-5cf6884e-ed2a-4eaf-b821-7179d7409376.png)

Or the one with more columns

```kusto
Resources
| where type == "microsoft.compute/virtualmachines"
| extend vmName = properties.osProfile.computerName
| extend osOffer = properties.storageProfile.imageReference.offer
| extend osSku = properties.storageProfile.imageReference.sku
| extend osName = properties.extended.instanceView.osName
| extend osVersion = properties.extended.instanceView.osVersion
| project id, resourceGroup, location, vmName, osOffer, osSku, osName, osVersion
```
![image](https://user-images.githubusercontent.com/96930989/211176924-227b2f7c-eaf1-47f2-a42e-e5b4d731b0ce.png)

Then download the csv file(from the first query with less columns), let's name it `table2`

### Join two tables together by matching data in the `id (resource id)` column  using `VLOOKUP` in excel
[How to use VLOOKUP to merge tables](https://www.youtube.com/watch?v=xjrZ4kwbh6w)

#### 1. Create a new excel file, paste `table1` and `table2` to the sheets in the same file.
![image](https://user-images.githubusercontent.com/96930989/211176981-883f82d7-984b-46a5-8199-39988c24bd04.png)
#### 2. In `table1`, insert a new column C named `OSSKU` ; then insert a new `row` on the top, input `5` above it
![image](https://user-images.githubusercontent.com/96930989/210538609-8ad0d2ec-5dd9-4f24-98b1-17b988dd9083.png)

#### 3. Navigate to `table2` which is in the same excel file, select all, and type a name in the section below, then press enter

In this demo, i will use the name `table_ingestion`
![image](https://user-images.githubusercontent.com/96930989/210542537-eb63b0c9-a728-4bc2-82c5-b88cb98c1d4d.png)


#### 4. Go back to `table1`, select `C3`, input `=VLOOKUP($B3,table_ingestion,C$1,FALSE`, press enter, the OS information should be filled automatically
![image](https://user-images.githubusercontent.com/96930989/210542709-2c182991-cd22-442e-ac0d-02a256aa4412.png)

#### 5. Expand the rest of the column, the values are supposed to be filled automatically

![image](https://user-images.githubusercontent.com/96930989/210542776-3f20b878-e4cd-4c0a-ad8a-b8914a4a3bdc.png)


## 9. ARG list DDOS related reports (including mitigation reports)
```kusto
AzureDiagnostics | whereCategory == "DDoSMitigationFlowLogs"
```

```kusto
AzureDiagnostics | whereCategory == "DDoSMitigationReports"
```

## 10. Check VA results
```kusto
securityresources | where type =~ "microsoft.security/assessments/subassessments"
        | extend assessmentKey=extract(@"(?i)providers/Microsoft.Security/assessments/([^/]*)", 1, id), subAssessmentId=tostring(properties.id), parentResourceId= extract("(.+)/providers/Microsoft.Security", 1, id)
        | extend resourceId = tostring(properties.resourceDetails.id)
        | extend subAssessmentName=tostring(properties.displayName),
            subAssessmentDescription=tostring(properties.description),
            subAssessmentRemediation=tostring(properties.remediation),
            subAssessmentCategory=tostring(properties.category),
            subAssessmentImpact=tostring(properties.impact),
            severity=tostring(properties.status.severity),
            status=tostring(properties.status.code),
            cause=tostring(properties.status.cause),
            statusDescription=tostring(properties.status.description),
            additionalData=tostring(properties.additionalData)
        | where assessmentKey == "1195afff-c881-495e-9bc5-1486211ae03f"
                      | where status == "Unhealthy"
        | summarize numOfResources=dcount(resourceId), timeGenerated=arg_max(todatetime(properties.timeGenerated), additionalData) by assessmentKey, subAssessmentId, subAssessmentName, subAssessmentCategory, severity, status, cause, statusDescription, subAssessmentDescription, subAssessmentRemediation, subAssessmentImpact
        | extend high = iff(severity == "High", 3,0), medium = iff(severity == "Medium", 2, 0), low = iff(severity == "Low", 1 ,0)
        | extend all = high + medium + low
        | order by all desc, numOfResources desc
```
