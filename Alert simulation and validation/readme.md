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
