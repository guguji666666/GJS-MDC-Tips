# MDE integration with defender for cloud

## Exclusion tag

To exclude VMs from MDE onboarding, apply this tag on the resource:

To exclude Azure machine from MDE onboarding, apply this tag on the resource: <br>
* Tag name: ExcludeMdeAutoProvisioning
* Tag value: True

To exclude AWS machine from MDE onboarding, apply this tag on the resource: <br>
* Tag name: ExcludeMdeAutoProvisioning
* Tag value: true

To exclude GCP machine from MDE onboarding, apply this tag on the resource: <br>
* Tag name: excludemdeautoprovisioning
* Tag value: true

For the existing VM already onboared to MDE, apply the tag `will not offboard` the machines from defender for endpoint. We need to: 
1. Add the tag to the existing machines which you don’t want to onboard to MDE <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/a9db568c-5625-4c61-9ef1-e0ed0aa0d72f)

2. Manually uninstall the MDE extensions from these machines in Azure portal
3. Offboard the machines using the local script downloaded from MDE panel
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/c33797b3-dcda-4f22-a7da-a092ae0eddd5)

### Add exclusion tag using powershell commands

[Apply tags with Azure PowerShell](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-resources-powershell)

```powershell
Set-ExecutionPolicy RemoteSigned

Connect-AzAccount -TenantId <tenant id>

Set-AzContext -Subscription <subscription id>

$tags = @{"ExcludeMdeAutoProvisioning"="True"}

$resource = Get-AzResource -Name <Arc VM name> -ResourceGroup <RG that Arc VM locates>

New-AzTag -ResourceId $resource.id -Tag $tags
```

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/5d14cf77-95a0-4e94-89de-d3dc27dac0ec)
