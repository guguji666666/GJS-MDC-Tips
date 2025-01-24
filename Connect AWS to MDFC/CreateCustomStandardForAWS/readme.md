# 1. List all data connectors in defender for cloud (subscription level)
```powershell
# Define parameters for the tenant ID and subscription ID
$tenantId = "<tenantid>" # input your tenant id
$subscriptionId = "<subid>" # input your subscription id

# Set the execution policy to RemoteSigned to allow this script to run
# Set-ExecutionPolicy -Scope Process -ExecutionPolicy RemoteSigned -Force

# Log in to the Azure account using the specified tenant ID
Connect-AzAccount -TenantId $tenantId

# Set the Azure context to the specified subscription
Set-AzContext -SubscriptionId $subscriptionId

# Retrieve the Azure access token and store it in a variable
$accessToken = (Get-AzAccessToken).Token

# Construct the URL for retrieving security connectors
$baseGetConnectorsUrl = "https://management.azure.com/subscriptions/$subscriptionId/providers/Microsoft.Security/securityConnectors"
$getConnectorsUrl = "$baseGetConnectorsUrl`?api-version=2024-03-01-preview"

# Send the GET request to retrieve the security connectors
$headers = @{
    Authorization = "Bearer $accessToken"
}
Write-Host "GET URL: $getConnectorsUrl"  # Debug output
$response = Invoke-RestMethod -Method Get -Uri $getConnectorsUrl -Headers $headers

# Define the path where you want to save the JSON file
$outputPath = "C:\temp\MDFC_Connectors_$subscriptionId.json"

# Convert the response to a well-formatted JSON string
$formattedJson = $response | ConvertTo-Json -Depth 100

# Write the formatted JSON to the file
$formattedJson | Out-File -FilePath $outputPath -Encoding utf8

Write-Host "Response has been exported to: $outputPath"

# Optionally, display the response in the console (for verification)
Write-Host "Response:"
$formattedJson | ConvertFrom-Json | Format-List *
```

### Sample output (note the parts below)
* Part 1 would be the resource group where the AWS data connector locates
* Part 2 would be the name of the AWS data connector locates
![image](https://github.com/user-attachments/assets/e90bc25a-2870-4b8f-b611-4735a20b8c16)



# 2. List existing standards for specified AWS data connector
```powershell
# Define parameters for the tenant ID, subscription ID, resource group, and AWS connector
$tenantId = "<tenantid>"
$subscriptionId = "<subid>"
$resourceGroup = "<RG_AWS_Connector>"
$awsConnector = "<AWS_Connector>"

# Set the execution policy to RemoteSigned to allow this script to run
Set-ExecutionPolicy -Scope Process -ExecutionPolicy RemoteSigned -Force

# Log in to the Azure account using the specified tenant ID
Connect-AzAccount -TenantId $tenantId

# Set the Azure context to the specified subscription
Set-AzContext -SubscriptionId $subscriptionId

# Retrieve the Azure access token and store it in a variable
$accessToken = (Get-AzAccessToken).Token

# Construct the URL for retrieving existing custom security standards
$baseGetStandardsUrl = "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.Security/securityConnectors/$awsConnector/providers/Microsoft.Security/securityStandards"
$getStandardsUrl = "$baseGetStandardsUrl`?api-version=2024-08-01"

# Send the GET request to retrieve the custom security standards
$headers = @{
    Authorization = "Bearer $accessToken"
}
Write-Host "GET URL: $getStandardsUrl"  # Debug output
$response = Invoke-RestMethod -Method Get -Uri $getStandardsUrl -Headers $headers

# Define the path where you want to save the JSON file
$outputPath = "C:\temp\securityStandards_awsconnector_$awsConnector.json"

# Convert the response to a well-formatted JSON string
$formattedJson = $response | ConvertTo-Json -Depth 100

# Write the formatted JSON to the file
$formattedJson | Out-File -FilePath $outputPath -Encoding utf8

Write-Host "Response has been exported to: $outputPath"

# Optionally, display the response in the console (for verification)
Write-Host "Response:"
$formattedJson | ConvertFrom-Json | Format-List *
```

### Sample output (note the assessment keys in request body from the response)
![image](https://github.com/user-attachments/assets/9f26fa67-2f06-46e7-a89d-0ee9929ab093)



# 2. Create Custom standard for AWS resources
Since we need to create same standard for other AWS account, use the script below:

```powershell
# Define parameters for the tenant ID, subscription ID, resource group, and AWS connector
$tenantId = "<tenantid>"
$subscriptionId = "<subid>"
$resourceGroup = "<RG_AWS_Connector>"
$awsConnector = "<AWS_Connector>"

# Set the execution policy to RemoteSigned to allow this script to run
# Set-ExecutionPolicy -Scope Process -ExecutionPolicy RemoteSigned -Force

# Log in to the Azure account using the specified tenant ID
Connect-AzAccount -TenantId $tenantId

# Set the Azure context to the specified subscription
Set-AzContext -SubscriptionId $subscriptionId

# Retrieve the Azure access token and store it in a variable
$accessToken = (Get-AzAccessToken).Token

# Clipboard utility to copy the access token for potential use elsewhere
$accessToken | Set-Clipboard

# Generate a new GUID for the custom security standard
$newStandardId = [guid]::NewGuid().Guid
$encodedStandardId = [System.Web.HttpUtility]::UrlEncode($newStandardId)

# Construct the URL for creating a new custom security standard for the AWS connector
$baseStandardUrl = "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.Security/securityConnectors/$awsConnector/providers/Microsoft.Security/securityStandards/$encodedStandardId"
$standardUrl = "$baseStandardUrl`?api-version=2024-08-01"

# Create the body for the PUT request to define the new custom security standard
# For the value inside assessmentKey, replace with the ones in request body we get from previous step
$standardBody = @{
    properties = @{
        displayName    = "20250124 GJS Test creating AWS custom standard using API gugugu"
        description    = ""
        standardType   = "Custom"
        assessments    = @(
            @{ assessmentKey = "5b3c2887-d7b7-4887-b074-4e6057027709" }
            @{ assessmentKey = "fe770214-7b47-48f7-a78c-1279c35d8279" }
            @{ assessmentKey = "7a152832-6600-49d1-89be-82e474190e13" }
        )
        cloudProviders = @("AWS")
    }
} | ConvertTo-Json -Depth 3

# Send the PUT request to create the custom security standard
$headers = @{
    Authorization = "Bearer $accessToken"
    "Content-Type" = "application/json"
}
Write-Host "Standard URL: $standardUrl"  # Debug output
Invoke-RestMethod -Method Put -Uri $standardUrl -Headers $headers -Body $standardBody

# Generate a new GUID for the security standard assignment
$newStandardAssignmentId = [guid]::NewGuid().Guid
$encodedAssignmentId = [System.Web.HttpUtility]::UrlEncode($newStandardAssignmentId)

# Construct the URL for creating a new custom security standard assignment for the AWS connector
$baseAssignmentUrl = "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.Security/securityConnectors/$awsConnector/providers/Microsoft.Security/standardAssignments/$encodedAssignmentId"
$assignmentUrl = "$baseAssignmentUrl`?api-version=2024-08-01"

# Create the body for the PUT request to assign the new custom security standard
$assignmentBody = @{
    properties = @{
        displayName      = "Assign custom standard for AWS resources"
        description      = "Assignment of custom standard for AWS resources"
        assignedStandard = @{
            id = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.Security/securityConnectors/$awsConnector/providers/Microsoft.Security/securityStandards/$newStandardId"
        }
        effect           = "Audit"
        excludedScopes   = @()
        exemptionData    = $null
        attestationData  = $null
    }
} | ConvertTo-Json -Depth 3

# Send the PUT request to create the security standard assignment
Write-Host "Assignment URL: $assignmentUrl"  # Debug output
Invoke-RestMethod -Method Put -Uri $assignmentUrl -Headers $headers -Body $assignmentBody
```
