# Useful scripts in defender for cloud

## 1.Remove legacy agent MMA and OMS agent from Azure VM at subscription level
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
## 2. Remove MDE extension from Azure VMs at the subsription level
```powershell
# Login into your account 
# Connect-AzAccount
# Subscription Id. The script run by each subscription
$subscriptionId = "<sub id>"
# The VM Name which you may not want to uninstall extension. Keep the first comma. 
$excludeVMNameList = ,"<vm1>","<vm2>"
# The extension Name which you want to uninstall extension. Keep the first comma.
# Added "MDE.Windows" and "MDE.Linux" to the list
$uninstallExtensionNameList = "MDE.Windows","MDE.Linux"

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
                    Write-Output "Failed to remove Extension $($ext.Name) for VM: $vmName"
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

## 3. List MDC assessments at subscription level
```powershell
# Define your Azure AD tenant ID, subscription ID, and optionally the resource group or resource ID
$tenantId = "<input your tenant id here>"
$subscriptionId = "<input your subscription id here>"
$scope = "/subscriptions/$subscriptionId"  # Modify this line if you need to scope down to a specific resource group or resource

# Authenticate with Azure and set the context
Connect-AzAccount -TenantId $tenantId
Set-AzContext -Subscription $subscriptionId

# Obtain the access token
$accessToken = (Get-AzAccessToken).Token

# Optionally copy the token to the clipboard for debugging purposes
# $accessToken | Set-Clipboard

# Define the API version and construct the API URI for Defender for Cloud Assessments
$apiVersion = "2020-01-01"
$uri = "https://management.azure.com/$scope/providers/Microsoft.Security/assessments?api-version=$apiVersion"

# Invoke the API Call
$response = Invoke-RestMethod -Method Get -Uri $uri -Headers @{ 'Authorization' = "Bearer $accessToken" }

# Convert the response to JSON if it's not already a PowerShell object
if ($response -is [string]) {
    $parsedResponse = $response | ConvertFrom-Json
} else {
    $parsedResponse = $response
}

# Save the response to a file named "list_mdc_assessments.json"
# Depth parameter is set to ensure all nested objects are fully captured
$parsedResponse | ConvertTo-Json -Depth 32 | Out-File -FilePath "list_mdc_assessments.json" -Force

# Check if the file was saved correctly
if (Test-Path "list_mdc_assessments.json") {
    Write-Output "The JSON response has been saved to list_mdc_assessments.json"
} else {
    Write-Error "Failed to save the JSON response to the file."
}

# Output the raw parsed response (Comment this line out if you don't want it displayed in PowerShell)
# $parsedResponse

# Loop through each assessment and output desired properties
foreach ($assessment in $parsedResponse.value) {
    $id = $assessment.id
    $displayName = $assessment.properties.displayName
    $statusCode = $assessment.properties.status.code

    Write-Output "Assessment ID: $id"
    Write-Output "Display Name: $displayName"
    Write-Output "Status Code: $statusCode"
    Write-Output "============================="
}

# End of script
```
