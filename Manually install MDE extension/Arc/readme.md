# Manually deploy MDE extension on Arc VM

##　Powershell scripts

### Windows

```powershell

[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12

# Install required powershell modules
Install-Module -Name Az.Accounts
Install-Module -Name Az.ConnectedMachine

# The command Get-AzConnectedMachine is part of Azure PowerShell module "Az.ConnectedMachine" and it is not installed. Run "Install-Module Az.ConnectedMachine" to install it.

Connect-AzAccount -TenantId <your tenant id>

Set-AzContext -Subscription <the subscription id where the arc vm locates>

$vm = Get-AzConnectedMachine -ResourceGroupName "<resource group>" -Name "<vm name>"

$mdePackage = Invoke-AzRestMethod -Uri https://management.azure.com/subscriptions/$($vm.id.split('/')[2])/providers/Microsoft.Security/mdeOnboardings/?api-version=2021-10-01-preview

# You can add forceReOnboarding = $true below if you want to force onboarding again
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


### Linux
```powershell

[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12

# Install required powershell modules
Install-Module -Name Az.Accounts
Install-Module -Name Az.ConnectedMachine

# The command Get-AzConnectedMachine is part of Azure PowerShell module "Az.ConnectedMachine" and it is not installed. Run "Install-Module Az.ConnectedMachine" to install it.

Connect-AzAccount -TenantId <your tenant id>

Set-AzContext -Subscription <the subscription id where the arc vm locates>

$vm = Get-AzConnectedMachine -ResourceGroupName "<resource group>" -Name "<vm name>"

$mdePackage = Invoke-AzRestMethod -Uri https://management.azure.com/subscriptions/$($vm.id.split('/')[2])/providers/Microsoft.Security/mdeOnboardings/?api-version=2021-10-01-preview

# You can add forceReOnboarding = $true below if you want to force onboarding again
$Setting = @{
    "azureResourceId" = $vm.Id
    "vNextEnabled" = $true
}
$protectedSetting = @{
    "defenderForEndpointOnboardingScript" = ($mdePackage.content | ConvertFrom-Json).value.properties.onboardingPackageWindows
}
# OS == Windows or Linux
New-AzConnectedMachineExtension -Name 'MDE.Linux' -ExtensionType 'MDE.Linux' -ResourceGroupName $vm.ResourceGroupName -MachineName $vm.Name -Location $vm.Location -Publisher 'Microsoft.Azure.AzureDefenderForServers' -Settings $Setting -ProtectedSetting $protectedSetting -AutoUpgradeMinorVersion -TypeHandlerVersion '1.0'
```


## API (Use postman)

### Linux VM

#### Get `<Base64EncodedPackage>` for Linux VM
##### 1. Navigate to [MDE portal](https://security.microsoft.com)
##### 2. Navigate to Settings > Endpoints
![image](https://user-images.githubusercontent.com/96930989/224611145-931e10e5-9929-448c-86c0-ec77ab850272.png)

##### 3. Select operating system: Linux Server
##### 4. Select Deployment method: Local Script
##### 5. Click “Download onboarding package“
![image](https://user-images.githubusercontent.com/96930989/224672377-386a0165-2bea-4e8e-aaae-f607b865ce46.png)

##### 6. Unzip/Extract the packaged onboarding script
![image](https://user-images.githubusercontent.com/96930989/224673057-7042f509-44ba-4113-8fd9-de081d681ffa.png)

Then create a new python file under the path, let's name it `MDELinux.py`

##### 7. Copy these python commands to `MDELinux.py`, then save it
```python
import base64
f = open('MicrosoftDefenderATPOnboardingLinuxServer.py', 'rb') 
text = f.read() 
f.close() 
base64_encoded_text = base64.b64encode(text)
with open('ouput.txt', 'w') as f:
   f.write(str(base64_encoded_text))
```

##### 8. Run the command
```python
cd <path of python files>
```
```python
py .\MDELinux.py
```
Sample <br>
![image](https://user-images.githubusercontent.com/96930989/224681648-8ac88c36-bfa2-4e75-8b80-f1fdcc7b7f15.png) <br>

You will then find the `output.txt` under that path, `remove` the leading characters `b'` from the front and the trailing `‘` at the end of the content of output.txt <br>  
![image](https://user-images.githubusercontent.com/96930989/224681935-6577228e-74ef-44b0-9964-ef6dfff87cf9.png) <br>  
![image](https://user-images.githubusercontent.com/96930989/224681985-513d76d7-5645-45b6-8e24-f792fc5fbf8d.png) <br>
  
##### 9. Copy the base64 code and paste it in the `<Base64EncodedPackage>` section  
##### 10. Send the request in the postman
![image](https://user-images.githubusercontent.com/96930989/224683919-d03d871c-2c62-46dd-a1bb-e47b0b8661ae.png)

##### 11. Check the MDE extension in Azure VM
![image](https://user-images.githubusercontent.com/96930989/224683242-4f7c0f47-2a56-4103-83e8-7857c0961f77.png)
