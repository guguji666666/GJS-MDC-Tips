## Dependency agent should be enabled for listed virtual machine images

[Powershell deployment](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/agent-dependency-windows#powershell-deployment)
```powershell
Set-AzVMExtension -ExtensionName "Microsoft.Azure.Monitoring.DependencyAgent" `
    -ResourceGroupName "<name of resource group of the VM>" `
    -VMName "<VM name>" `
    -Publisher "Microsoft.Azure.Monitoring.DependencyAgent" `
    -ExtensionType "DependencyAgentWindows" `
    -TypeHandlerVersion 9.10 `
    -Location <Location of VM>
```
