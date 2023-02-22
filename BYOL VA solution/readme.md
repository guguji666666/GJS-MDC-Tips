## Steps to deploy BYOL VA solution
[Deploy a bring your own license (BYOL) vulnerability assessment solution](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-byol-vm)

### Configure a new BYOL solution in your subscription
1. Defender for cloud -> recommendation -> Machines should have a vulnerability assessment solution -> fix
![image](https://user-images.githubusercontent.com/96930989/213363188-c2164819-4b1e-4ac3-8362-c21c0f00025a.png)

2. Click `Configure a new third-party vulnerability scanner (BYOL - requires a separate license)`
![image](https://user-images.githubusercontent.com/96930989/213363249-0c70bfe7-f470-432f-9457-46f7cacbd8c3.png)

3. Fill your license code and public key.
![image](https://user-images.githubusercontent.com/96930989/213363258-3e164de5-cb0a-46f1-95cb-03ccc3d90dc7.png)


### Apply existing BYOL solution to your VM
1. Defender for cloud -> recommendation -> Machines should have a vulnerability assessment solution -> fix
![image](https://user-images.githubusercontent.com/96930989/213402182-bfa3d63a-c330-4304-9000-cb2249601855.png)

2. Click `Deploy your configured third-party vulnerability scanner (BYOL - requires a separate license)`

3. Proceed


### Migrate the VM to new BYOL solution
1. Turn off auto-provisioning in existing BYOL solution
![image](https://user-images.githubusercontent.com/96930989/215679440-2b832156-b5be-49ba-ab19-91707fc54d15.png)
![image](https://user-images.githubusercontent.com/96930989/215679303-90b07626-ce66-49ee-beb0-005122ffb93d.png)
![image](https://user-images.githubusercontent.com/96930989/215679331-bd66f99f-b077-4ecf-b916-b6a227c87315.png)

2. Remove VM from existing BYOL solution(Using postman)

`Binding` - DELETE

`URL`
```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{RGName}/providers/Microsoft.Security/locations/{subscriptionLocation}/securitySolutions/{solutionName}/protectedResources/{azureResourceId of VM}?api-version=2015-06-01-preview
```

3. Manually uninstall the extension in Azure
4. Ensure there are no folders named ”qualys” under /etc
5. Add the VM to the new BYOL solution


### Useful API for BYOL VA solution

#### 1. List existing BYOL solutions activated in your subscription
Binding
```
GET
```
URL
```
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Security/securitySolutions?api-version=2015-06-01-preview
```
![image](https://user-images.githubusercontent.com/96930989/220620651-981a2f19-ae6b-4e3b-a5fa-dd9abf625e84.png)

You can also find the location of the subscription from the results
![image](https://user-images.githubusercontent.com/96930989/220620754-5942bca4-3981-4c74-bdab-dad757ad6633.png)


#### 2. List all resourcess(VM) linked to the specified BYOL VA solution
Binding
```
GET
``` 
URL
```
https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{RG of the BYOL solution }/providers/Microsoft.Security/locations/{subscriptionLocation}/securitySolutions/{SolutionName}?api-version=2015-06-01-preview&includeProtectedResources=true
```
![image](https://user-images.githubusercontent.com/96930989/220621676-cc185327-aa7a-4457-a3dd-5066912dd281.png)


#### 3. Delete an existing VA BYOL Solution from the subscription
Binding
```
DELETE
``` 
URL
```
https://management.azure.com/subscriptions/{subscritionId}/resourceGroups/{RG of BYOL solution}/providers/Microsoft.Security/locations/{ASCLocation}/securitySolutions/{solutionName}/?api-version=2015-06-01-preview
```

#### 4. Add specified VM to the existing BYOL VA solution
Binding
```
PUT
```
URL
```
https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{RG of BYOL solution}/providers/Microsoft.Security/locations/{subscriptionLocation}/securitySolutions/{SolutionName}/protectedResources?api-version=2015-06-01-preview 
```
Body
```
[{ ResourceAzureId: "{vm Resource Id}" }]
```

#### 5. Remove specified VM from the BYOL VA solution
Binding
```
Delete
```
URL
```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{RGName of BYOL solution}/providers/Microsoft.Security/locations/{subscriptionLocation}/securitySolutions/{solutionName}/protectedResources/{azureResourceId of VM}?api-version=2015-06-01-preview 
```


