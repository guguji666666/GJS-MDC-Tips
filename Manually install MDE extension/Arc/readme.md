# Manually deploy MDE extension on Arc VM

## Windows

```powershell

[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12

#Install required powershell modules
Install-Module -Name Az.Accounts
Install-Module -Name Az.ConnectedMachine

#The command Get-AzConnectedMachine is part of Azure PowerShell module "Az.ConnectedMachine" and it is not installed. Run "Install-Module Az.ConnectedMachine" to install it.

Connect-AzAccount -TenantId <your tenant id>

Set-AzContext -Subscription <the subscription id where the arc vm locates>

$vm = Get-AzConnectedMachine -ResourceGroupName <resource group> -Name <vm name>

$mdePackage = Invoke-AzRestMethod -Uri https://management.azure.com/subscriptions/$($vm.id.split('/')[2])/providers/Microsoft.Security/mdeOnboardings/?api-version=2021-10-01-preview

#You can add forceReOnboarding = $true below if you want to force onboarding again
$Setting = @{
    "azureResourceId" = $vm.Id
    "vNextEnabled" = $true
}
$protectedSetting = @{
    "defenderForEndpointOnboardingScript" = ($mdePackage.content | ConvertFrom-Json).value.properties.onboardingPackageWindows
}
# OS == Windows or Linux
New-AzConnectedMachineExtension -Name 'MDE.Windows' -ExtensionType 'MDE.Windows' -ResourceGroupName $vm.ResourceGroupName -MachineName $vm.Name -Location $vm.Location -Publisher 'Microsoft.Azure.AzureDefenderForServers' -Settings $Setting -ProtectedSetting $protectedSetting -AutoUpgradeMinorVersion -TypeHandlerVersion '1.0'
```
