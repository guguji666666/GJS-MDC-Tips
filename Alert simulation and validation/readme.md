# Alert simulation and validation

## [Alert validation in Microsoft Defender for Cloud](https://learn.microsoft.com/en-us/azure/defender-for-cloud/alert-validation)

## Defender for key vault
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
$vaultUri = 'https://<vault-name>.vault.azure.net'  # Replace <vault-name> with your vault's name
$secretName = '<secret-name>'
$iterations = 10  # Number of iterations

# Loop to simulate secret listing and query activities without pausing
for ($i = 1; $i -le $iterations; $i++) {
    # Simulate a secret listing activity
    $secrets = Get-AzKeyVaultSecret -VaultName $vaultUri

    Write-Host "Iteration $($i): Secret Listing Activity Detected"
    Write-Host "Number of Secrets Listed: $($secrets.Count)"
    Write-Host "---------------------------------"

    # Simulate a secret query activity
    $secretValue = Get-AzKeyVaultSecret -VaultName $vaultUri -Name $secretName

    Write-Host "Iteration $($i): Secret Query Activity Detected"
    Write-Host "Secret Value: $($secretValue.SecretValueText)"
    Write-Host "---------------------------------"
}

```
