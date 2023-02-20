## Symptom: Azure databricks storage accounts could not exempted from the recommendation
## Below would be the information required before reaching microsoft support

### Use ARG - Resource Graph Explorer
#### 1. Navigate to Azure portal, and search `Resource Graph Explorer` on the top
![image](https://user-images.githubusercontent.com/96930989/210159757-b875ba41-6946-4ee7-a604-92183cf9f58b.png)

#### 2. Find `ASSESSMENT-KEY` from recommendation
![image](https://user-images.githubusercontent.com/96930989/220023467-58dfa81b-dec8-4433-a7f1-60e904e5e7b5.png)

![image](https://user-images.githubusercontent.com/96930989/220023598-38f83a87-83c8-4b39-801c-126fafcb2a0b.png)

#### 3. Find `POLICY-DEFINITION-ID` from recommendation
![image](https://user-images.githubusercontent.com/96930989/220023678-c5cf5fcc-70f5-4b51-8d92-095d38804981.png)
![image](https://user-images.githubusercontent.com/96930989/220023825-b0dc8c2a-310f-447f-99ad-ec2d20bf43eb.png)

#### 4. Copy the last part of the `Definition ID`
![image](https://user-images.githubusercontent.com/96930989/220023758-6dd08272-9568-4bbc-b3c6-03cd81ba45bf.png)

### Get unhealthy databricks storage accounts (you can also set the filter for subscription)
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
        | where assessmentKey == "<assessment key you found from the recommendation>"
        //| where subscriptionId == "<sub id>"
        | where resourceId contains "databricks"
        | project tenantId, subscriptionId, resourceId
```


### Get all databricks workspaces in the tenant (you can also set the filter for subscription)
```kusto
// Run query to see results.
where type == "microsoft.databricks/workspaces"
| project id,name,type,resourceGroup,location,subscriptionId,kind,tags
| extend typeDisplayName=case(type =~ 'microsoft.databricks/workspaces','Azure Databricks Service',type)
| extend locationDisplayName=case(location =~ 'eastus','East US',location =~ 'eastus2','East US 2',location =~ 'southcentralus','South Central US',location =~ 'westus2','West US 2',location =~ 'westus3','West US 3',location =~ 'australiaeast','Australia East',location =~ 'southeastasia','Southeast Asia',location =~ 'northeurope','North Europe',location =~ 'swedencentral','Sweden Central',location =~ 'uksouth','UK South',location =~ 'westeurope','West Europe',location =~ 'centralus','Central US',location =~ 'southafricanorth','South Africa North',location =~ 'centralindia','Central India',location =~ 'eastasia','East Asia',location =~ 'japaneast','Japan East',location =~ 'koreacentral','Korea Central',location =~ 'canadacentral','Canada Central',location =~ 'francecentral','France Central',location =~ 'germanywestcentral','Germany West Central',location =~ 'norwayeast','Norway East',location =~ 'switzerlandnorth','Switzerland North',location =~ 'uaenorth','UAE North',location =~ 'brazilsouth','Brazil South',location =~ 'qatarcentral','Qatar Central',location =~ 'centralusstage','Central US (Stage)',location =~ 'eastusstage','East US (Stage)',location =~ 'eastus2stage','East US 2 (Stage)',location =~ 'northcentralusstage','North Central US (Stage)',location =~ 'southcentralusstage','South Central US (Stage)',location =~ 'westusstage','West US (Stage)',location =~ 'westus2stage','West US 2 (Stage)',location =~ 'asia','Asia',location =~ 'asiapacific','Asia Pacific',location =~ 'australia','Australia',location =~ 'brazil','Brazil',location =~ 'canada','Canada',location =~ 'europe','Europe',location =~ 'france','France',location =~ 'germany','Germany',location =~ 'global','Global',location =~ 'india','India',location =~ 'japan','Japan',location =~ 'korea','Korea',location =~ 'norway','Norway',location =~ 'singapore','Singapore',location =~ 'southafrica','South Africa',location =~ 'switzerland','Switzerland',location =~ 'uae','United Arab Emirates',location =~ 'uk','United Kingdom',location =~ 'unitedstates','United States',location =~ 'unitedstateseuap','United States EUAP',location =~ 'eastasiastage','East Asia (Stage)',location =~ 'southeastasiastage','Southeast Asia (Stage)',location =~ 'northcentralus','North Central US',location =~ 'westus','West US',location =~ 'jioindiawest','Jio India West',location =~ 'westcentralus','West Central US',location =~ 'southafricawest','South Africa West',location =~ 'australiacentral','Australia Central',location =~ 'australiacentral2','Australia Central 2',location =~ 'australiasoutheast','Australia Southeast',location =~ 'japanwest','Japan West',location =~ 'jioindiacentral','Jio India Central',location =~ 'koreasouth','Korea South',location =~ 'southindia','South India',location =~ 'westindia','West India',location =~ 'canadaeast','Canada East',location =~ 'francesouth','France South',location =~ 'germanynorth','Germany North',location =~ 'norwaywest','Norway West',location =~ 'switzerlandwest','Switzerland West',location =~ 'ukwest','UK West',location =~ 'uaecentral','UAE Central',location =~ 'brazilsoutheast','Brazil Southeast',location)
| extend subscriptionDisplayName=case(subscriptionId =~ 'd681dc2c-9d21-4b07-aa03-28016ca5dff4','Jiashenggu-Test',subscriptionId)
| where (type !~ ('microsoft.azureactivedirectory/ciamdirectories'))
| where (type !~ ('microsoft.agfoodplatform/farmbeats'))
| where (type !~ ('microsoft.cdn/profiles/customdomains'))
| where (type !~ ('microsoft.cdn/profiles/afdendpoints'))
| where (type !~ ('microsoft.cdn/profiles/origingroups/origins'))
| where (type !~ ('microsoft.cdn/profiles/origingroups'))
| where (type !~ ('microsoft.cdn/profiles/afdendpoints/routes'))
| where (type !~ ('microsoft.cdn/profiles/rulesets/rules'))
| where (type !~ ('microsoft.cdn/profiles/rulesets'))
| where (type !~ ('microsoft.cdn/profiles/secrets'))
| where (type !~ ('microsoft.cdn/profiles/securitypolicies'))
| where (type !~ ('microsoft.codesigning/codesigningaccounts'))
| where (type !~ ('microsoft.kubernetes/connectedclusters/microsoft.kubernetesconfiguration/namespaces'))
| where (type !~ ('microsoft.containerservice/managedclusters/microsoft.kubernetesconfiguration/namespaces'))
| where (type !~ ('microsoft.kubernetes/connectedclusters/microsoft.kubernetesconfiguration/fluxconfigurations'))
| where (type !~ ('microsoft.containerservice/managedclusters/microsoft.kubernetesconfiguration/fluxconfigurations'))
| where (type !~ ('microsoft.portalservices/extensions/deployments'))
| where (type !~ ('microsoft.portalservices/extensions'))
| where (type !~ ('microsoft.portalservices/extensions/slots'))
| where (type !~ ('microsoft.portalservices/extensions/versions'))
| where (type !~ ('microsoft.databaseinsights/databasemonitoringagents'))
| where (type !~ ('microsoft.datacollaboration/workspaces'))
| where (type !~ ('microsoft.documentdb/mongoclusters'))
| where (type !~ ('microsoft.eventgrid/namespaces'))
| where (type !~ ('microsoft.hdinsight/clusterpools/clusters'))
| where (type !~ ('microsoft.hdinsight/clusterpools'))
| where (type !~ ('microsoft.network/virtualhubs')) or ((kind =~ ('routeserver')))
| where (type !~ ('microsoft.metaverse/metaverses'))
| where (type !~ ('microsoft.modsimworkbench/instances/chambers'))
| where (type !~ ('microsoft.modsimworkbench/instances/chambers/connectors'))
| where (type !~ ('microsoft.modsimworkbench/instances/chambers/files'))
| where (type !~ ('microsoft.modsimworkbench/instances/chambers/filerequests'))
| where (type !~ ('microsoft.modsimworkbench/instances/chambers/licenses'))
| where (type !~ ('microsoft.modsimworkbench/instances/chambers/storages'))
| where (type !~ ('microsoft.modsimworkbench/instances/chambers/workloads'))
| where (type !~ ('microsoft.modsimworkbench/instances/sharedstorages'))
| where (type !~ ('microsoft.insights/diagnosticsettings'))
| where (type !~ ('microsoft.insights/scheduledqueryrules'))
| where not((type =~ ('microsoft.network/serviceendpointpolicies')) and ((kind =~ ('internal'))))
| where (type !~ ('microsoft.openlogisticsplatform/workspaces'))
| where (type !~ ('microsoft.scom/managedinstances'))
| where (type !~ ('microsoft.orbital/edgesites'))
| where (type !~ ('microsoft.orbital/groundstations'))
| where (type !~ ('microsoft.orbital/l2connections'))
| where (type !~ ('microsoft.workloads/phpworkloads'))
| where (type !~ ('microsoft.recommendationsservice/accounts/modeling'))
| where (type !~ ('microsoft.recommendationsservice/accounts/serviceendpoints'))
| where (type !~ ('microsoft.recoveryservicesbvtd/vaults'))
| where (type !~ ('microsoft.recoveryservicesbvtd2/vaults'))
| where (type !~ ('microsoft.recoveryservicesintd/vaults'))
| where (type !~ ('microsoft.recoveryservicesintd2/vaults'))
| where (type !~ ('microsoft.deploymentmanager/rollouts'))
| where (type !~ ('microsoft.datareplication/replicationvaults'))
| where (type !~ ('microsoft.storagecache/amlfilesystems'))
| where (type !~ ('microsoft.storage/storagetasks'))
| where not((type =~ ('microsoft.synapse/workspaces/sqlpools')) and ((kind =~ ('v3'))))
| where (type !~ ('microsoft.windowspushnotificationservices/registrations'))
| where not((type =~ ('microsoft.sql/servers/databases')) and ((kind in~ ('system','v2.0,system','v12.0,system','v12.0,user,datawarehouse,gen2,analytics'))))
| where not((type =~ ('microsoft.sql/servers')) and ((kind =~ ('v12.0,analytics'))))
//| where subscriptionId == "<your sub id>"
| project name, subscriptionId, id
```


