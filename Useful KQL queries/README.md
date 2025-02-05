# Use ARG - Resource Graph Explorer

Navigate to Azure portal, and search `Resource Graph Explorer` on the top <br>
![image](https://user-images.githubusercontent.com/96930989/210159757-b875ba41-6946-4ee7-a604-92183cf9f58b.png)

## [Azure Resource Graph sample queries for Microsoft Defender for Cloud](https://learn.microsoft.com/en-us/azure/defender-for-cloud/resource-graph-samples?tabs=azure-cli)

## ARG list all subscriptions under your tenant

```kusto
resourcecontainers
| where type == "microsoft.resources/subscriptions"
| project name, id
```

```kusto
resourcecontainers
| where type == "microsoft.resources/subscriptions"
| extend subscriptionId = split(id, "/")[2]
| project name, subscriptionId
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/39a6e992-e96d-43f5-96da-b2e433844bf9)

## ARG list all subscriptions under specified management group
```kusto
resourcecontainers
| where type == 'microsoft.resources/subscriptions'
| mv-expand managementGroupParent = properties.managementGroupAncestorsChain
| where managementGroupParent.name =~ '<name of management group>'
| project name, id
| sort by name asc
```

## List all security connectors (AWS,GCP) connected to defender for cloud
```kusto
resources 
| where ['type'] =~ "microsoft.security/securityconnectors"
| project id, name, tenantId
```
![image](https://github.com/user-attachments/assets/f85f3377-a65b-4b14-8512-c074acd4e5b4)

```kusto
resources 
| where ['type'] =~ "microsoft.security/securityconnectors"
| project ConnectorResourceID = id, ConnectorName = name, tenantId, EnvironmentType = properties.environmentData.environmentType, AccountID = properties.hierarchyIdentifier
```
![image](https://github.com/user-attachments/assets/f2a11240-b0cf-41ac-8bad-e96b6cff22c1)


```kusto
resources
| where type == "microsoft.security/securityconnectors"
| extend id = id  // Extracting id directly before packing
| extend connector = 
    iff(
        type == "microsoft.security/securityconnectors", 
        pack(
            "id", id,
            "location", location,
            "type", type,
            "name", name,
            "tags", tags,
            "kind", kind,
            "properties", properties
        ), 
        dynamic(null)
    )
| extend connectorDisplayName = 
    iff(
        type == "microsoft.security/securityconnectors", 
        strcat(tostring(properties.hierarchyIdentifier), " (", name, ")"), 
        dynamic(null)
    )
| extend connector = pack("connector", connector, "displayName", connectorDisplayName)
| extend 
    environmentType = tostring(properties.environmentData.environmentType),  // Directly from properties
    hierarchyIdentifier = tostring(properties.hierarchyIdentifier),  // Directly from properties
    displayName = tostring(connector.displayName)
| project subscriptionId, id, environmentType, hierarchyIdentifier, displayName
| order by id
```
![image](https://github.com/user-attachments/assets/763d38c4-5471-4c11-b961-66f6dbf48849)


## List all VM extensions and provisioning status

Azure VM
```kusto
resources
| where type == "microsoft.compute/virtualmachines/extensions"
| extend VM = tostring (split(id, "/")[8])
| extend Extension = name
| project VM, Extension, Publisher = properties.publisher, Status = properties.provisioningState
| order by VM
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/5a02ef47-60ff-4d43-8820-48f81d851437)

Arc VM
```kusto
resources
| where type == "microsoft.hybridcompute/machines/extensions"
| extend VM = tostring (split(id, "/")[8])
| extend Extension = name
| project VM, Extension, Publisher = properties.publisher, Status = properties.provisioningState
| order by VM
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/2d9d028b-f636-46d8-a07c-563e06daf5dd)


## List VMs with `SqlIaasExtension` installed and provisioning status
#### SQL servers on machines

For Azure VM
```kusto
resources
| where type == "microsoft.compute/virtualmachines/extensions"
| extend VM = tostring (split(id, "/")[8])
| extend Extension = name
| where Extension contains "SqlIaasExtension"
| project VM, Extension, Publisher = properties.publisher, Status = properties.provisioningState
```

For Arc VM
```kusto
resources
| where type == "microsoft.hybridcompute/machines/extensions"
| extend VM = tostring (split(id, "/")[8])
| extend Extension = name
| where Extension contains "SqlIaasExtension"
| project VM, Extension, Publisher = properties.publisher, Status = properties.provisioningState
```


## List status of OMS/MMA and AMA status on VMs
```kusto
resources
| where type == "microsoft.hybridcompute/machines/extensions"or type == "microsoft.compute/virtualmachines/extensions"
| extend VM = tostring (split(id, "/")[8])
| extend Extension = name
| where Extension in ("MicrosoftMonitoringAgent","OmsAgentForLinux","AzureMonitorLinuxAgent","AzureMonitorWindowsAgent")
| project id, VM, Extension, Publisher = properties.publisher, Status = properties.provisioningState
| order by VM
```

![image](https://github.com/user-attachments/assets/91808aed-dcf0-46ed-9774-32167fa1c1fc)


## List defender plans status of each subscription

```kusto
SecurityResources
| where type == 'microsoft.security/pricings'
| extend subId = tostring(subscriptionId)
| project subId, Azure_Defender_plan= name, Status= properties.pricingTier
| join kind = leftouter 
(
resourcecontainers
| where type == "microsoft.resources/subscriptions"
| extend subId = tostring (split(id, "/")[2])
| project name, subId
) on subId
| project name, subId, Azure_Defender_plan, Status
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/0e226f4c-ec41-4f5d-bb99-cede8e030c79)

## Check status of MDE extension on Azure VM and Arc VM

Azure VM
```kusto
resources
| where type == "microsoft.compute/virtualmachines/extensions"
| extend VM = tostring (split(id, "/")[8])
| extend Extension = name
| where Extension contains "MDE"
| project VM, Extension, Publisher = properties.publisher, Status = properties.provisioningState
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/a1da4588-8cec-40dd-84bb-cb33b799595f)

Arc VM
```kusto
resources
| where type == "microsoft.hybridcompute/machines/extensions"
| extend VM = tostring (split(id, "/")[8])
| extend Extension = name
| where Extension contains "MDE"
| project VM, Extension, Publisher = properties.publisher, Status = properties.provisioningState
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/b6d5384b-42d0-4809-81db-e28d410147ee)


## ARG list current secure score of all subscriptions

```kusto
SecurityResources 
| where type == 'microsoft.security/securescores' 
| extend current = properties.score.current, max = todouble(properties.score.max)
| project subscriptionId, current, max, percentage = ((current / max)*100)
```

## ARG list all initiatives in tenant
```kusto
policyresources
| where type == "microsoft.authorization/policysetdefinitions"
| project displayName = properties.displayName, policySetDefinitionId=name
```
![image](https://github.com/user-attachments/assets/588a7565-d1cf-4c1b-9289-0522d231bd04)


## ARG check relevant initiatives in subscription (basic)

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

## ARG check relevant initiatives in subscription (advanced)

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

## ARG check exemptions in MDC
```kusto
securityresources
| where type == "microsoft.security/assessments"
| where subscriptionId == "<your subscription id>"
| extend initiatives = properties.statusPerInitiative
| extend  RecommendationName = properties.displayName
| extend  ResourceName = split(id, "/")[8]
| mv-expand initiatives
| extend initiativeName = initiatives.policyInitiativeName
| extend statusInMdc = initiatives.assessmentStatus.code
| where statusInMdc contains "NotApplicable"
| extend reason = properties.status.cause
| project initiativeName, statusInMdc, RecommendationName, ResourceName, reason
| where initiativeName contains "Microsoft cloud security benchmark" or initiativeName contains "AWS Foundational Security Best Practices"
| where reason contains "exempt"
```
![image](https://github.com/user-attachments/assets/e854c870-6ff3-483f-86c9-32ba984211f6)


```kusto
securityresources
| where type == "microsoft.security/assessments"
| where subscriptionId == "<sub id>"
| extend initiatives = properties.statusPerInitiative
| extend RecommendationName = properties.displayName
| extend ResourceName = split(id, "/")[8]
| mv-expand initiatives
| extend initiativeName = initiatives.policyInitiativeName
| extend statusInMdc = initiatives.assessmentStatus.code
| where statusInMdc contains "NotApplicable"
| extend reason = properties.status.cause
| extend description = properties.status.description
| project initiativeName, statusInMdc, RecommendationName, ResourceName, reason, description
| where initiativeName contains "Microsoft cloud security benchmark" or initiativeName contains "ASC"
| where reason contains "exempt"
// | summarize count() by tostring(RecommendationName)
```
![image](https://github.com/user-attachments/assets/ebb418bd-22d7-4c13-9e2f-9e7085eb509b)


## ARG compare results between Defender for cloud and Azure Policy
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

## ARG list all unpatched VM along with OS information
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


## ARG list DDOS related reports (including mitigation reports)
```kusto
AzureDiagnostics | whereCategory == "DDoSMitigationFlowLogs"
```

```kusto
AzureDiagnostics | whereCategory == "DDoSMitigationReports"
```

## Check Vulnerability findings
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

```kusto
securityresources
| where type == "microsoft.security/assessments/subassessments"
| extend assessmentKey = extract(".*assessments/(.+?)/.*",1,  id)
| where assessmentKey == "1195afff-c881-495e-9bc5-1486211ae03f"
| project Resource = tolower(extract("([\\s\\S]*?)(/providers/Microsoft.Security.*)",1,id)), ResourceGroup = trim_end("/",extract(".*resourceGroups/(.+?)/",0,id)), ResourceType = tolower(split(id,"/").[6]), subscriptionId, Severity = tostring(parse_json(properties).status.severity), Status = tostring(parse_json(properties).status.code), VulnId = tostring(parse_json(properties).id), Description = tostring(parse_json(properties).displayName), Patchable = parse_json(properties.additionalData).patchable, CVE = properties.additionalData.cve, Category = tostring(properties.category), TimeGenerated = tostring(properties.timeGenerated), Remediation = tostring(properties.remediation), Impact = tostring(properties.impact), Threat = tostring(properties.additionalData.threat)
| where Status == 'Unhealthy'
//| where '{selectedServer}' == 'All' or Resource == '{selectedServer}'
| project Severity, VulnId, Description, tostring(Patchable), Category, Resource, ResourceGroup, CVE, TimeGenerated, Remediation, Impact, Threat
| mv-expand CveExpand = split (CVE, "},") to typeof(string)
| parse CveExpand with * '"title":"' singleCve '"' *
| summarize CVEs = tostring(make_list(singleCve)) by Severity, VulnId, Description, tostring(Patchable), Category, Resource, ResourceGroup, TimeGenerated, Threat, Impact, Remediation
```

```kusto
securityresources 
| where type == "microsoft.security/assessments/subassessments"
| extend assessmentKey = extract(@"assessments/([a-f0-9-]+)", 1, id),
         machineName = extract(".*virtualMachines/(.+?)/.*", 1, id), 
         Description = tostring(parse_json(properties).displayName),
         Severity = tostring(parse_json(properties).status.severity)
| join kind=inner ( 
    securityresources  
    | where type =~ "microsoft.security/assessments"
    | extend assessmentKey = extract(@"assessments/([a-f0-9-]+)", 1, id),
             resourceType = tolower(properties.resourceDetails.ResourceType),
             status = tolower(tostring(properties.status.code)),
             riskFactors = iff(type == "microsoft.security/assessments", 
                               iff(isnull(properties.risk.riskFactors), dynamic([]), properties.risk.riskFactors), 
                               dynamic(null))        
    | where resourceType contains "microsoft.compute/virtualmachines"
    | where status == "unhealthy"
    | where set_has_element(riskFactors, "Vulnerabilities")
    | project assessmentKey
) on assessmentKey
| extend TimeGenerated = properties.timeGenerated,
         CVE = properties.additionalData.cve
| project machineName, Description, CVE, properties, TimeGenerated
```


## ARG Summarize the count of vulnerabilities at the repo level (defender for container)
```kusto
securityresources
| where type == "microsoft.security/assessments/subassessments"
| extend assessmentKey = extract(".*assessments/(.+?)/.*",1,  id)
| where assessmentKey == "c0b7cfc6-3172-465a-b378-53c7ff2cc0d5"
| project Resource = tolower(extract(@'(?i)(.*?)/providers/Microsoft.Security/([^/]+)', 1, id)), ResourceType = tolower(split(id,"/").[6]), subscriptionId, severity = tostring(parse_json(properties).additionalData.vulnerabilityDetails.severity), status = tostring(parse_json(properties).status.code), VulnId = tostring(parse_json(properties).id), description = tostring(parse_json(properties).displayName), patchable = parse_json(properties.additionalData).softwareDetails.fixStatus, cve = parse_json(properties.additionalData).cve, imageURI = tostring(parse_json(properties.resourceDetails).id)
| where status == 'Unhealthy'
| extend Registry = tostring(split(Resource, "/").[-1]), Repo = tostring(split(imageURI, "/")[2])
| summarize dcount(VulnId) by Resource, severity, VulnId, description, tostring(patchable), tostring(cve), Repo, Registry, imageURI
| summarize Total = count(dcount_VulnId), sevCritical=countif(severity=='Critical'), sevHigh=countif(severity=='High'), sevMedium=countif(severity=='Medium'), sevLow=countif(severity=='Low'), patchAvailable = countif(patchable=='FixAvailable'), CVEcount =countif(cve!='[]') by Resource, Registry, Repo, imageURI
| project-away Resource
| order by sevCritical desc
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/f813a615-93bf-4866-9c75-ba8a02addf05)


## ARG get list of CVE findings via repo and contains description (defender for container)
```kusto
securityresources
| where type =~ "microsoft.security/assessments/subassessments"
| extend assessmentKey=extract(@"(?i)providers/Microsoft.Security/assessments/([^/]*)", 1, id)
| where assessmentKey == "c0b7cfc6-3172-465a-b378-53c7ff2cc0d5"
| where properties.status.code == "Unhealthy"
| extend parentResourceId = extract(@"(?i)(.*/Microsoft.ContainerRegistry/registries/[^/]*)", 1, id)
| extend resourceId = tostring(properties.resourceDetails.id)
| extend parsedAdditionalData = parse_json(properties.additionalData)
| extend cveId = tostring(parsedAdditionalData.vulnerabilityDetails.cveId),
        subAssessmentDescription=tostring(properties.description),
        subAssessmentRemediation=tostring(properties.remediation),
        subAssessmentCategory=tostring(parsedAdditionalData.softwareDetails.category),
        severity = tostring(parsedAdditionalData.vulnerabilityDetails.severity),
        status=tostring(properties.status.code)
| extend CVELink = iif(isnotempty(parsedAdditionalData.vulnerabilityDetails.references), tostring(parsedAdditionalData.vulnerabilityDetails.references[0].link), ""),
          registryHost = tostring(parsedAdditionalData.artifactDetails.registryHost),
          repositoryName = tostring(parsedAdditionalData.artifactDetails.repositoryName),
          fixReference = tostring(parsedAdditionalData.softwareDetails.fixReference)
| summarize numOfResources=dcount(resourceId), timeGenerated=arg_max(todatetime(properties.timeGenerated), properties.additionalData) by assessmentKey, cveId, subAssessmentCategory, severity, status, subAssessmentDescription, subAssessmentRemediation, CVELink, registryHost, repositoryName, fixReference
| extend critical = iff(severity == "Critical", 5,0),
        high = iff(severity == "High", 4,0),
        medium = iff(severity == "Medium", 3, 0),
        low = iff(severity == "Low", 2 ,0),
        unknown = iff(severity == "Unknown", 1, 0)
| extend all = critical + high + medium + low + unknown
| order by all desc, numOfResources desc
| project cveId, registryHost, repositoryName, severity, status, subAssessmentDescription, subAssessmentRemediation, CVELink, fixReference
| order by repositoryName
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/25c378c3-6fb7-43b2-8e7e-9c3fcbfa87e6)


## Monitor protection status
```kusto
ProtectionStatus
| distinct Computer
```

```kusto
ProtectionStatus
| summarize LastCall = max(TimeGenerated) by Computer
| order by LastCall asc 
```


