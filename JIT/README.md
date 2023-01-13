## Create JIT policy for VM
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

