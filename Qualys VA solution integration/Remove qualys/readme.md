# Uninstall qualys from VM

Log in to the Windows VM and execute following command:
C:\Program Files\qualys\qualysagent\uninstall.exe" Uninstall=True Force=True
Execute the following commands:
sc query qualysagent
sc query qmon


```powershell
Write-Output "Remove qualys agent folder from the location C:\ProgramData\Qualys"
Remove-Item -Path "C:\ProgramData\Qualys" -Recurse -Force

Write-Output "Remove the QualysAgent from below provided registry"
Remove-Item -Path "HKLM:\SYSTEM\CurrentControlSet\services\QualysAgent" -Recurse -Force
Remove-Item -Path "HKLM:\SYSTEM\ControlSet001\Services\QualysAgent" -Recurse -Force
Remove-Item -Path "HKLM:\SYSTEM\ControlSet002\Services\QualysAgent" -Recurse -Force

Write-Output "Remove Qualys from below registry location"
Remove-Item -Path "HKLM:\SOFTWARE\Qualys" -Recurse -Force

Write-Output "Remove B0276D68D7BC3C348BC53A5177FFA6CE from below registry location"
Remove-Item -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Installer\UserData\S-1-5-18\Products\B0276D68D7BC3C348BC53A5177FFA6CE" -Recurse -Force
Remove-Item -Path "HKLM:\SOFTWARE\Classes\Installer\Products\B0276D68D7BC3C348BC53A5177FFA6CE" -Recurse -Force

Write-Output "Display message indicating that all traces have been removed from the target machine"
Write-Output "All traces of Qualys Agent have been removed from the target machine."
```
