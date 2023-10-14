# Alert simulation and validation

## [Alert validation in Microsoft Defender for Cloud](https://learn.microsoft.com/en-us/azure/defender-for-cloud/alert-validation)

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/36bb9540-1a36-45b8-b2bc-3a24bada5a4e)

## Defender for key vault
### 1. [Download tor browser](https://www.torproject.org/download/)
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/730dc6bb-2af3-4900-9c47-484c2535d84a)

### 2. Run powershell script in tor  browser to get secrets



```powershell
# AAD Auth
Connect-AzAccount -subscription <subscription id>

# Set your Azure Key Vault name
$KeyVaultName = "<your-unique-keyvault-name>"
$SecretName = "ExamplePassword"
$NumberOfIterations = 3000  # Change this to the desired number of iterations

# Loop through the command for the specified number of iterations
for ($i = 1; $i -le $NumberOfIterations; $i++) {
    try {
        $secret = Get-AzKeyVaultSecret -VaultName $KeyVaultName -Name $SecretName -AsPlainText
        Write-Host ("Iteration {0}: Secret value is {1}" -f $i, $secret)
    } catch {
        Write-Host ("Error occurred in iteration {0}: {1}" -f $i, $_.Exception.Message)
    }
}
```

```powershell
# Authenticate to Azure
Connect-AzAccount

# Define your Key Vault details
$vaultName = 'Contoso'  # Replace with your Key Vault name
$secretName = 'secret1'
$iterations = 3000  # Number of iterations

# Loop to simulate secret query activity without pausing
for ($i = 1; $i -le $iterations; $i++) {
    # Simulate a secret query activity
    $secretValue = Get-AzKeyVaultSecret -VaultName $vaultName -Name $secretName

    Write-Host "Iteration $($i): Secret Query Activity Detected"
    Write-Host "Secret Value: $($secretValue.SecretValueText)"
    Write-Host "---------------------------------"
}
```
