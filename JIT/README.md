# JIT to VM
## [Permissions required](https://learn.microsoft.com/en-us/azure/defender-for-cloud/just-in-time-access-overview?tabs=defender-for-container-arch-aks#faq---just-in-time-virtual-machine-access)



## P1 : Create JIT policy for VM
Example - Enable just-in-time VM access on a specific VM with the following rules:
* Close ports 22 and 3389
* Set a maximum time window of 6 hours for each so they can be opened per approved request
* Allow the user who is requesting access to control the source IP addresses
* Allow the user who is requesting access to establish a successful session upon an approved just-in-time access request

### 1. Install module Az.Security
```powershell
Set-PSRepository PSGallery -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned

Install-Module Az.Security

Import-Module -Name "Az.Security" -Verbose -Force
```

### 2. Assign a variable that holds the just-in-time VM access rules for a VM
```powershell

Connect-AzAccount -Subscription <your subscription id>

$JitPolicy = (@{
    id="/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME";
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

Set-AzJitNetworkAccessPolicy -Kind "Basic" -Location "LOCATION" -Name "default" -ResourceGroupName "RESOURCEGROUP" -VirtualMachine $JitPolicyArr
```

## P2 : Create Custom role for requiring JIT access (least privileged)
### 1. Creating the template in json file
To add Azure custom RBAC Role you'll need to create a `.json` file with the "actions" and "non-actions" defined.

Create a json file with this content
```json
{
    "Name":  "JIT Custom Role",
    "Id":  "88888888-8888-8888-8888-888888888888",
    "IsCustom":  true,
    "Description":  "Enable user to request JIT access with restricted privileges",
    "Actions":  [
        "Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action",
        "Microsoft.Security/locations/jitNetworkAccessPolicies/*/read",
        "Microsoft.Compute/virtualMachines/read",
        "Microsoft.Network/networkInterfaces/*/read"],
    "NotActions":  [
  
                   ],
    "DataActions":  [
  
                    ],
    "NotDataActions":  [
  
                       ],
    "AssignableScopes":  [
                             "/subscriptions/<SubscriptionID>"]
}
```

### 2. Create the custom in subscription
```powershell
Set-PSRepository PSGallery -InstallationPolicy Trusted

Install-Module -Name "Az"

Set-ExecutionPolicy RemoteSigned

Import-Module -Name "Az" -Verbose -Force

Connect-AzAccount -subscription <subscription id>

New-AzRoleDefinition -InputFile <path to the json file you created before>
```

To verify if the custom role has been created successfully
```powershell
Get-AzRoleDefinition -name "JIT Custom Role"

Get-AzRoleDefinition| ? {$_.IsCustom -eq$true} | FT Name, IsCustom

Get-AzRoleDefinition| ? {$_.IsCustom -eq$true} | fl
```

Check the new custom role in subscription's IAM management (below is the sample)
![image](https://user-images.githubusercontent.com/96930989/212226060-93f41f7f-baed-49c3-957f-2d8a96a98616.png)

## P3 : Request JIT access to VM
Configure the VM request access properties
```powershell
Connect-AzAccount -Subscription <subscription id>

$JitPolicyVm1 = (@{
    id="/subscriptions/<SUBSCRIPTIONID>/resourceGroups/<RESOURCEGROUP>/providers/Microsoft.Compute/virtualMachines/<VMNAME>";
    ports=(@{
       number=<port>;
       endTimeUtc="<2020-07-15T17:00:00.3658798Z>";
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
