# Manage JIT rule on Azure VM
1. [What is JIT?](https://learn.microsoft.com/en-us/azure/defender-for-cloud/just-in-time-access-overview?tabs=defender-for-container-arch-aks)
2. [Permissions required](https://learn.microsoft.com/en-us/azure/defender-for-cloud/just-in-time-access-overview?tabs=defender-for-container-arch-aks#faq---just-in-time-virtual-machine-access)
3. [Enable JIT on your VMs using PowerShell](https://learn.microsoft.com/en-us/azure/defender-for-cloud/just-in-time-access-usage?tabs=jit-config-powershell%2Cjit-request-powershell#powershell)

## P1 : Create JIT policy for VM
Below is the `example` - Enable just-in-time VM access on a specific VM with the following rules:
* Close ports `22` and `3389`
* Set a maximum time window of `6 hours` for each so they can be opened per approved request
* Allow the user who is requesting access to control the `source IP addresses`
* Allow the user who is requesting access to establish a successful session upon an approved just-in-time access request

### 1. Install `Azure Az` powershell module 
```powershell
Set-PSRepository PSGallery -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned

Install-Module Az
```
[Install the Azure Az PowerShell module](https://learn.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-9.3.0)

### 2. Create JIT rule and assign it to the VM
```powershell

Connect-AzAccount -Subscription <your subscription id>

$JitPolicy = (@{
    id="/subscriptions/<SUBSCRIPTIONID>/resourceGroups/<RESOURCEGROUP>/providers/Microsoft.Compute/virtualMachines/<VMNAME>";
    ports=(@{
         number=22;
         protocol="*";
         allowedSourceAddressPrefix=@("*");
         maxRequestAccessDuration="PT6H"},
         @{
         number=3389;
         protocol="*";
         allowedSourceAddressPrefix=@("*");
         maxRequestAccessDuration="PT6H"})})

$JitPolicyArr=@($JitPolicy)

Set-AzJitNetworkAccessPolicy -Kind "Basic" -Location "<LOCATION of VM>" -Name "default" -ResourceGroupName "<RESOURCEGROUP>" -VirtualMachine $JitPolicyArr
```
Sample:
```
Connect-AzAccount -Subscription 8b7ef460-1d5d-41ef-b73c-ccfe136cb315

$JitPolicy = (@{
    id="/subscriptions/8b7ef460-1d5d-41ef-b73c-ccfe136cb315/resourceGroups/GJS-MS150-MDFC1/providers/Microsoft.Compute/virtualMachines/gjsubuntu2004lts01";
    ports=(@{
         number=22;
         protocol="*";
         allowedSourceAddressPrefix=@("*");
         maxRequestAccessDuration="PT6H"},
         @{
         number=3389;
         protocol="*";
         allowedSourceAddressPrefix=@("*");
         maxRequestAccessDuration="PT6H"})})

$JitPolicyArr=@($JitPolicy)

Set-AzJitNetworkAccessPolicy -Kind "Basic" -Location "East Asia" -Name "default" -ResourceGroupName "GJS-MS150-MDFC1" -VirtualMachine $JitPolicyArr
```
![image](https://user-images.githubusercontent.com/96930989/212462373-95aae3a2-f926-4c19-a2df-62b8b32e694f.png)

![image](https://user-images.githubusercontent.com/96930989/212462411-e07876ed-4a16-4f32-8e7e-8e6695812fb2.png)

## P2 : Create custom role for requiring JIT access (least privileged)
### 1. Create the role template in json file
To add Azure custom RBAC Role you'll need to create a `.json` file with the `"actions"` and `"non-actions"` defined.

Create a json file with this content
```
Note:
"Id" can be left all 8's for the submission, you will get a new one generated on Addition in the results.
```

User can request JIT via `API` or `UI` with the least permission below
```json
{
    "Name":  "JIT least privileges",
    "Id":  "88888888-8888-8888-8888-888888888888",
    "IsCustom":  true,
    "Description":  "Enable user to request JIT access with restricted privileges",
    "Actions":  [
        "Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action",
        "Microsoft.Security/locations/jitNetworkAccessPolicies/*/read",
        "Microsoft.Compute/virtualMachines/read",
        "Microsoft.Network/networkInterfaces/*/read",
        "Microsoft.Network/publicIPAddresses/read"],
    "NotActions":  [
  
                   ],
    "DataActions":  [
  
                    ],
    "NotDataActions":  [
  
                       ],
    "AssignableScopes":  [
                             "/subscriptions/<subscription id>"]
}
```

#### If you want the user to request JIT only to `specified VM` , then we need to:
Create `Custom role 1` to set permission on the network resources, assign it to the subscription level
```json
{
    "Name":  "JIT least privileges without VM read",
    "Id":  "88888888-8888-8888-8888-888888888888",
    "IsCustom":  true,
    "Description":  "Enable user to request JIT access with restricted privileges",
    "Actions":  [
        "Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action",
        "Microsoft.Security/locations/jitNetworkAccessPolicies/*/read",
        "Microsoft.Network/networkInterfaces/*/read",
        "Microsoft.Network/publicIPAddresses/read"],
    "NotActions":  [
  
                   ],
    "DataActions":  [
  
                    ],
    "NotDataActions":  [
  
                       ],
    "AssignableScopes":  [
                             "/subscriptions/<subscription id>"]
}
```

Create `Custom role 2` to set restrcition so that user can only see specified VM only, type `resource id` of the VM at the `AssignableScopes` section
```json
{
    "Name":  "See only <name of VM>",
    "Id":  "88888888-8888-8888-8888-888888888888",
    "IsCustom":  true,
    "Description":  "User can only see specified VM",
    "Actions":  [
        "Microsoft.Compute/virtualMachines/read"],
    "NotActions":  [
  
                   ],
    "DataActions":  [
  
                    ],
    "NotDataActions":  [
  
                       ],
    "AssignableScopes":  [
                             "/subscriptions/<resource id of the VM>"]
}
```
You can also assign the reader/contributor permission directly to user in the IAM page of specified VM


Check IAM at VM level
* Assign custom role `JIT least privileges without VM read` to user at the subscription level
![image](https://user-images.githubusercontent.com/96930989/212807829-064d976b-da04-47c1-922d-624f4627a48c.png)
![image](https://user-images.githubusercontent.com/96930989/212807880-16f1d6b6-be2d-486a-be26-b01dbb8c55c8.png)

* Assign read permission at the `VM level` only, user can only see the VM which he has the read permission
![image](https://user-images.githubusercontent.com/96930989/212705984-f3a6f6c6-0bed-4e9c-a000-59fe46bc4704.png)


#### If you want the user to request JIT from security portal, then we need to:
Create `Custom role 1` for network permission at subscription level
```json
{
    "Name":  "JIT Full network",
    "Id":  "88888888-8888-8888-8888-888888888888",
    "IsCustom":  true,
    "Description":  "Enable user to request JIT access with restricted privileges",
    "Actions":  [
        "Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action",
        "Microsoft.Security/locations/jitNetworkAccessPolicies/*/read",
        "Microsoft.Compute/virtualMachines/read",
        "Microsoft.Network/*/read",
        "Microsoft.Security/policies/read",
        "Microsoft.Security/pricings/read"],
    "NotActions":  [
  
                   ],
    "DataActions":  [
  
                    ],
    "NotDataActions":  [
  
                       ],
    "AssignableScopes":  [
                             "/subscriptions/<subscription id>"]
}
```


### 2. Create the custom role in subscription 
```powershell
Connect-AzAccount -subscription <subscription id>
```
```powershell
New-AzRoleDefinition -InputFile <path to the json file you created before>
```
Sample

![image](https://user-images.githubusercontent.com/96930989/212461351-d404a10d-f515-496a-94bf-dbb21f73ccef.png)

Methods to deploy custom role in Azure
* [Create or update Azure custom roles using the Azure portal](https://learn.microsoft.com/en-us/azure/role-based-access-control/custom-roles-portal)
* [Tutorial: Create an Azure custom role using Azure CLI](https://learn.microsoft.com/en-us/azure/role-based-access-control/tutorial-custom-role-cli)
* [Tutorial: Create an Azure custom role using Azure PowerShell](https://learn.microsoft.com/en-us/azure/role-based-access-control/tutorial-custom-role-powershell)
* [Create or update Azure custom roles using the REST API](https://learn.microsoft.com/en-us/azure/role-based-access-control/custom-roles-rest)
* [Create or update Azure custom roles using an ARM template](https://learn.microsoft.com/en-us/azure/role-based-access-control/custom-roles-template)

### 3. To `verify` if the custom role has been created successfully
```powershell
Connect-AzAccount -subscription <subscription id>
```
```powershell
Get-AzRoleDefinition -name "JIT Custom Role"
```
![image](https://user-images.githubusercontent.com/96930989/212461418-8110dcaf-6231-4f9b-a993-37b349bfc56d.png)

or
```powershell
Get-AzRoleDefinition| ? {$_.IsCustom -eq$true} | FT Name, IsCustom
```
![image](https://user-images.githubusercontent.com/96930989/212461435-622d716c-a927-4905-bae7-c85d10a77abf.png)

or
```powershell
Get-AzRoleDefinition| ? {$_.IsCustom -eq$true} | fl
```
![image](https://user-images.githubusercontent.com/96930989/212461447-81eacada-8aa8-4b69-8c95-cdfd752047b2.png)

You can also check the new custom role in subscription's IAM management (below is the sample)
![image](https://user-images.githubusercontent.com/96930989/212461506-2b833d9d-c1f8-4d69-a733-d309ebfd476d.png)


## P3 : Request JIT access to VM using powershell commands
Configure the VM request access properties
```powershell
Connect-AzAccount -Subscription <subscription id>

$JitPolicyVm1 = (@{
    id="/subscriptions/<SUBSCRIPTIONID>/resourceGroups/<RESOURCEGROUP>/providers/Microsoft.Compute/virtualMachines/<VMNAME>";
    ports=(@{
       number=<port>;
       endTimeUtc="2020-07-15T17:00:00.3658798Z";
       allowedSourceAddressPrefix=@("<IPV4ADDRESS of the source>")})})       
```

Insert the VM access request parameters in an array
```powershell
$JitPolicyArr=@($JitPolicyVm1)
```

Send the request access (use the resource ID from step 1)

```powershell
Start-AzJitNetworkAccessPolicy -ResourceId "/subscriptions/<sub id>/resourceGroups/<RG name>/providers/Microsoft.Security/locations/<location of VM>/jitNetworkAccessPolicies/default" -VirtualMachine $JitPolicyArr
```
Sample
![image](https://user-images.githubusercontent.com/96930989/212462460-3e19164e-f9c4-46f7-898f-2d1a509f992b.png)

The JIT rule should be found in UI
![image](https://user-images.githubusercontent.com/96930989/212462523-9b961667-a51f-4051-b035-a773120cb498.png)

If you want to check the definition inside JIT rule you just created
1. Navigate to `defender for cloud panel > Workload protections`
2. On the right, click `Just-in-time VM access` under `Advanced protection`
![image](https://user-images.githubusercontent.com/96930989/212526103-d5bdd1f9-2f74-4785-8330-40c40971d094.png)
3. You can see the VM with configured JIT rule, right click and select `Edit`
![image](https://user-images.githubusercontent.com/96930989/212526195-d4b9944d-5a8d-47a5-bb0b-b2245aa583af.png)
4. You can then see the definition of this JIT rule
![image](https://user-images.githubusercontent.com/96930989/212526219-f751d997-a572-42b5-b17b-98d16ebb2d45.png)

# JIT FAQ
## 1. If i enable JIT in the UI, What is the name of this JIT policy?
We picked Azure VM running Linux for the test, after we enabled JIT in UI
![image](https://user-images.githubusercontent.com/96930989/212584226-29ce3adc-9983-4673-96f8-a50f64f7208d.png)

Check the JIT policy using powershell commands
```powershell
Connect-AzAccount -Subscription <subscription id>
Get-AzJitNetworkAccessPolicy
```
The name of the JIT policy is `default`
![image](https://user-images.githubusercontent.com/96930989/212585689-59fa7d3f-9422-4ff0-9057-4a1ea9e9d525.png)

