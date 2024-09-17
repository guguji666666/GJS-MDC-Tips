# Useful scripts in defender for cloud

## 1.Remove legacy agent MMA and OMS agent at subscription level
``` powershell
# Login into your account 
# Connect-AzAccount
# Subscription Id. The script run by each subscription
$subscriptionId = "<sub id>"
# The VM Name which you may not want to uninstall extension. Keep the first comma. 
$excludeVMNameList = ,"<vm1>","<vm2>"
# The entension Name which you want to uninstall extension. Keep the first comma.
$uninstallExtensionNameList = ,"MMAExtension","MicrosoftMonitoringAgent","OmsAgentForLinux","OMSExtension"
 
Select-AzSubscription -SubscriptionId $subscriptionId
$vmList = Get-AzVM
foreach($vm in $vmList){
    $vmName = $vm.Name
    $vmLocation = $vm.Location
    if($excludeVMNameList -icontains $vmName){
        Write-Output "VM: $vmName is in the excludeVMNameList, skip"
        Continue
    }
    $vmExtensions = (Get-AzVM -ResourceGroupName $vm.ResourceGroupName -Name $vmName -Status).Extensions
    $isAnyExtensionProcessed = $false
    if(($vmExtensions -ne $null) -and ($vmExtensions.Count -gt 0)) {
        foreach($ext in $vmExtensions) {
            if($uninstallExtensionNameList -icontains $ext.Name) {
                $isAnyExtensionProcessed = $true
                Write-Output "Removing Extension $($ext.Name) for VM: $vmName"
                try{
                    Remove-AzVMExtension -ResourceGroupName $vm.ResourceGroupName -Name $ext.Name -VMName $vmName -Force -ErrorAction SilentlyContinue
                    Write-Output "Successfully removed Extension $($ext.Name) for VM: $vmName"
                }catch{
                    Write-Output "Failed to removed Extension $($ext.Name) for VM: $vmName"
                    Write-Output $_
                }
            }
        }
        if($isAnyExtensionProcessed -eq $false){
            Write-Output "VM: $vmName does not have any extension need to be uninstalled by the uninstallExtensionNameList, skip."
        }
    }else{
        Write-Output "VM: $vmName does not have any extension or is not running, skip."
    }
}
```
