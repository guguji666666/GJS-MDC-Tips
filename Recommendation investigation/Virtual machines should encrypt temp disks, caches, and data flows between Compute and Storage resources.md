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

Notice

Enabling both ADE and HBE at the same time on a VM is not supported at present. We should only enable either ADE or HBE.

##### [Comparison of Disk Storage SSE, ADE, encryption at host, and Confidential disk encryption](https://learn.microsoft.com/en-us/azure/virtual-machines/disk-encryption-overview#comparison)

![image](https://user-images.githubusercontent.com/96930989/229993443-7b8961a6-da20-440e-a059-f247ff9e7ec1.png)


1.Check Azure Disk Encryption on the VM

[Quickstart: Create and encrypt a Windows virtual machine in Azure with PowerShell](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/disk-encryption-powershell-quickstart)

[Create a Key Vault configured for encryption keys](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/disk-encryption-powershell-quickstart#create-a-key-vault-configured-for-encryption-keys)

```powershell
Set-ExecutionPolicy RemoteSigned
Install-Module Az
```
```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
Connect-AzAccount -TenantId <your tenant id>
Set-AzContext -Subscription <subscription id>
```
```powershell
New-AzKeyvault -name <name of your KV> -ResourceGroupName <Name of your resource group> -Location EastUS -EnabledForDiskEncryption
```

Sample:
```powershell
New-AzKeyvault -name GJSADEKV -ResourceGroupName ADEKV -Location EastAsia -EnabledForDiskEncryption
```
![image](https://user-images.githubusercontent.com/96930989/230056186-da3cf419-f97e-4ab8-917e-8d9fdf6fc818.png)

Then, we can [Encrypt the virtual machine](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/disk-encryption-powershell-quickstart#encrypt-the-virtual-machine)
```powershell
$KeyVault = Get-AzKeyVault -VaultName MyKV -ResourceGroupName MyResourceGroup
```
```powershell
Set-AzVMDiskEncryptionExtension -ResourceGroupName <Name of the resource group where VM locates> -VMName <Name of the VM> -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri -DiskEncryptionKeyVaultId $KeyVault.ResourceId
```
Sample
```powershell
$KeyVault = Get-AzKeyVault -VaultName MyKV -ResourceGroupName MyResourceGroup
```
```powershell
Set-AzVMDiskEncryptionExtension -ResourceGroupName <Name of the resource group where VM locates> -VMName <Name of the VM> -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri -DiskEncryptionKeyVaultId $KeyVault.ResourceId


ADE extension


Check ADE state on disk
