## Guest configuration 

### 1. Steps to reinstall guest configuration extension

#### 1. Uninstall guest configuration from Azure VM
![image](https://user-images.githubusercontent.com/96930989/233086469-31dcaf22-3cd0-4af5-bb43-8ef785a78ec1.png)

#### 2. Add Service tags  to port 80 and 443
* AzureArcInfrastructure
* Storage
* GuestAndHybridManagement

Sample (`Service tag` AzureArcInfrastructure, `Port`80) <br>
<img width="1929" alt="image" src="https://user-images.githubusercontent.com/96930989/233061489-abb59dd6-4ee1-45b7-b2a0-dad24b980fd4.png">

#### 3. Enable System assigned identity 

##### Remove files from local machine

C:\ProgramData  <br>
<img width="814" alt="image" src="https://user-images.githubusercontent.com/96930989/233085903-dd13f0b4-f2d9-4d0d-9576-98b8a985a2ba.png">

C:\Packages\Plugins  <br>
<img width="936" alt="image" src="https://user-images.githubusercontent.com/96930989/233087257-af642ef1-fa85-4a2e-bc3e-5bb8990c0834.png">

C:\WindowsAzure\Logs\Plugins  <br>
<img width="981" alt="image" src="https://user-images.githubusercontent.com/96930989/233087464-de4c3737-30d7-46a6-91db-8b271054d2ba.png">

#### 5. Reboot the machine
#### 6. Run Azure cloudshell command to trigger manual scan
```powershell
$job = Start-AzPolicyComplianceScan -AsJob
```

### 2. Check reports in guest assignments UI

#### 1. Search `Guest assignments` in Azure portal
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/4140907e-d59c-45d4-9a94-5bdc16a665ea)

Here you can see the overview of the guest assignments <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/f6794782-37dd-4304-9b1d-ae6132ba961a)

You can also set the filter for `Subscription`, `Virtual machine` to get the results of specified VM <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/1cbfa251-c210-4f1d-aeff-9dc014734925)


### 3. Check the security findings for specified VM in recommendation 
#### Sample
Vulnerabilities in security configuration on your Linux machines should be remediated (powered by Guest Configuration) <br>
Click `Affected resources` > `Unhealthy resources`, search for the VM you want to investigate <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/a5806778-4728-43a3-8a2b-194826d3e51f)

Then you will see the findings from this VM <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/5b0e48f3-83f5-49ba-8598-626d8cffc1d6)


### 4. Check the findings for specified VM using ARG
Click the button here <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/760c55b6-920b-4ea4-b8e6-2e7a1ea8adce)

Then you will find the overview results of all VMs <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/dff3710d-4c57-4ba4-9876-c0a59e2464c5)

If you want to filter results for specified VM, use the ARG query below <br>
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
        | where assessmentKey == "1f655fb7-63ca-4980-91a3-56dbc2b715c6"
                      | where status == "Unhealthy"  
                  | project resourceId, assessmentKey, subAssessmentName
        | where resourceId contains "<VM name>"   //Input your VM name here
```

Sample
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
        | where assessmentKey == "1f655fb7-63ca-4980-91a3-56dbc2b715c6"
                      | where status == "Unhealthy"  
                  | project resourceId, assessmentKey, subAssessmentName
        | where resourceId contains "CEF"
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/afb7c523-ccd5-4090-b9e1-3717637f708f)

