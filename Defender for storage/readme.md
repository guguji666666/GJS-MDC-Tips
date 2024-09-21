# Defender for storage

## Check defender for storage plan at `subscription` level

### Check the type of defender for storage plan

```kusto
securityresources 
| where type == "microsoft.security/pricings"
| where subscriptionId == "<your subscription id>"
| where ['id'] contains "storage"
| project subscriptionId, id, properties.pricingTier, properties.subPlan
```

![image](https://user-images.githubusercontent.com/96930989/230877193-f43bda35-a282-48f0-b3c2-df8543d15a04.png)

![image](https://user-images.githubusercontent.com/96930989/230875611-5a3abf0b-6c47-480d-bfe5-20fe76bf1dc6.png)


### Deploy defender for storage plan with per-transaction pricng mode

We are able to exclude a specific Storage account if the legacy `"Per transaction"` pricing plan is enabled

We CAN'T exclude a specific Storage account if the legacy `"Per-storage-account plan"` (new)" pricing plan is enabled

Refer customer to Can I exclude specific storage accounts from protections in per-storage account pricing? 

[Set up per-transaction pricing for a subscription](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-storage-classic-enable#set-up-microsoft-defender-for-storage-classic)

[Exclude an Azure Storage account protection on a subscription with per-transaction pricing](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-storage-classic-enable#exclude-an-azure-storage-account-protection-on-a-subscription-with-per-transaction-pricing)


## Check defender for storage plan at `resource` level

### Powershell script > output in `table` view
```powershell
# Ensure the Azure PowerShell module is installed
# Install-Module -Name Az -AllowClobber -Force
# Log in to the Azure account; this will prompt you to authenticate
Connect-AzAccount -Tenant <tenant id>
# (Optional) Select the desired subscription if you have multiple
$desiredSubscriptionId = "<subscription id>"
Select-AzSubscription -SubscriptionId $desiredSubscriptionId
# Retrieve the current context and the subscription ID
$context = Get-AzContext
$subId = $context.Subscription.Id
# Retrieve all storage accounts in the current subscription context
$SAs = Get-AzStorageAccount
# Get the access token as a SecureString
$secureAccessToken = (Get-AzAccessToken -AsSecureString).Token
# Convert the SecureString to a plain string
$accessToken = [System.Net.NetworkCredential]::new("", $secureAccessToken).Password
# Array to hold storage account details for table view
$StorageAccountTable = @()
# Loop through each storage account to check Defender for Storage settings
foreach ($SA in $SAs) {
    # Construct the resource ID for the storage account
    $resID = "/subscriptions/$subId/resourceGroups/$($SA.ResourceGroupName)/providers/Microsoft.Storage/storageAccounts/$($SA.StorageAccountName)"
    
    # Construct the API URL for checking Defender for Storage settings
    $apiUrl = "https://management.azure.com$resID/providers/Microsoft.Security/defenderForStorageSettings/current?api-version=2022-12-01-preview"
    
    # Call the API to get the Defender for Storage settings
    $defenderSettings = Invoke-RestMethod -Uri $apiUrl -Method Get -Headers @{ "Authorization" = "Bearer $accessToken" } -UseBasicParsing | Select-Object -ExpandProperty properties
    
    # Prepare the default values for the table row
    $classicPlan = $false
    $newPlan = $false
    # Check if Defender for Storage is Enabled
    if ($defenderSettings.isEnabled -eq $true) {
        $newPlan = $true
    } else {
        # If Defender for Storage is Disabled, check the advanced threat protection settings
        $advancedThreatProtection = Get-AzSecurityAdvancedThreatProtection -ResourceId $resID
        
        if ($advancedThreatProtection.IsEnabled -eq $true) {
            $classicPlan = $true
        }
    }
    # Add the result to the table array
    $StorageAccountTable += [PSCustomObject]@{
        ResourceId   = $resID
        ClassicPlan  = $classicPlan
        NewPlan      = $newPlan
    }
}
# Output the results in a table format
$StorageAccountTable | Format-Table -AutoSize
```
![image](https://github.com/user-attachments/assets/55c66d3d-6574-4cb2-b255-d95b84abec9c)


### Powershell script > output in `json` view
```powershell
# Ensure the Azure PowerShell module is installed
# Install-Module -Name Az -AllowClobber -Force
# Log in to the Azure account; this will prompt you to authenticate
Connect-AzAccount -Tenant <tenant id>
# (Optional) Select the desired subscription if you have multiple
$desiredSubscriptionId = "<subscription id>"
Select-AzSubscription -SubscriptionId $desiredSubscriptionId
# Retrieve the current context and the subscription ID
$context = Get-AzContext
$subId = $context.Subscription.Id
# Retrieve all storage accounts in the current subscription context
$SAs = Get-AzStorageAccount
# Get the access token as a SecureString
$secureAccessToken = (Get-AzAccessToken -AsSecureString).Token
# Convert the SecureString to a plain string
$accessToken = [System.Net.NetworkCredential]::new("", $secureAccessToken).Password
# Array to hold storage account details for JSON output
$StorageAccountList = @()
# Loop through each storage account to check Defender for Storage settings
foreach ($SA in $SAs) {
    # Construct the resource ID for the storage account
    $resID = "/subscriptions/$subId/resourceGroups/$($SA.ResourceGroupName)/providers/Microsoft.Storage/storageAccounts/$($SA.StorageAccountName)"
    
    # Construct the API URL for checking Defender for Storage settings
    $apiUrl = "https://management.azure.com$resID/providers/Microsoft.Security/defenderForStorageSettings/current?api-version=2022-12-01-preview"
    
    # Call the API to get the Defender for Storage settings
    $defenderSettings = Invoke-RestMethod -Uri $apiUrl -Method Get -Headers @{ "Authorization" = "Bearer $accessToken" } -UseBasicParsing | Select-Object -ExpandProperty properties
    
    # Prepare the default values for the storage account status
    $classicPlan = $false
    $newPlan = $false
    # Check if Defender for Storage is Enabled
    if ($defenderSettings.isEnabled -eq $true) {
        $newPlan = $true
    } else {
        # If Defender for Storage is Disabled, check the advanced threat protection settings
        $advancedThreatProtection = Get-AzSecurityAdvancedThreatProtection -ResourceId $resID
        
        if ($advancedThreatProtection.IsEnabled -eq $true) {
            $classicPlan = $true
        }
    }
    # Add the result to the list
    $StorageAccountList += [PSCustomObject]@{
        ResourceId   = $resID
        ClassicPlan  = $classicPlan
        NewPlan      = $newPlan
    }
}
# Convert the list to JSON format and output
$StorageAccountList | ConvertTo-Json -Depth 10 | Write-Output
```
![image](https://github.com/user-attachments/assets/a2a03676-417d-4aeb-9398-4436456b88ab)





