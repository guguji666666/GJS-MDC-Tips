# Alert simulation and validation

## [Alert validation in Microsoft Defender for Cloud](https://learn.microsoft.com/en-us/azure/defender-for-cloud/alert-validation)

## Defender for key vault
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/36bb9540-1a36-45b8-b2bc-3a24bada5a4e)

### 1. [Download tor browser](https://www.torproject.org/download/)
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/730dc6bb-2af3-4900-9c47-484c2535d84a)

### 2. Run powershell script in tor  browser to get secrets

Navigate to [Azure cloudshell](https://ms.portal.azure.com/#cloudshell/) and sign in

Run the scirpt below
```powershell
# AAD Auth
Connect-AzAccount -subscription <subscription id>

# Set your Azure Key Vault name
$KeyVaultName = "<keyvault-name>"
$SecretName = "<secret name>"
$NumberOfIterations = 8000  # Change this to the desired number of iterations

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
* The script will get secret values for 8000 times, which may take some time
* Once the script ends, it may take 1 hour to generate defender for key vault alert

Alert i received <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/16962491-7494-4bb1-8a73-f97c9a0c7ea6)

Alert in defender for cloud portal <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/2570417b-43b5-4b0c-b65d-d44897feb96d)

Alert : Access from a TOR exit node to a key vault <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/771f8c5c-7f84-428a-99bb-cc1538eae7de)

Alert : Security incident detected suspicious Key Vault activity (Preview) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/ad76dc7c-53c0-4a33-b162-7c839849b1b4)

Alert : Security incident detected suspicious IP activity (Preview) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/4912e800-57ee-4371-8969-2bb05e3f9917)


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
