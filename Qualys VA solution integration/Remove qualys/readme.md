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


```powershell
# Stop QualysAgent service
$serviceName = 'QualysAgent'
if (Get-Service $serviceName -ErrorAction SilentlyContinue) {
    Stop-Service -Name $serviceName
    Write-Host "Stopped service $serviceName."
}

# Uninstall Qualys Cloud Security Agent
$AppName = "Qualys Cloud Security Agent"
$App = Get-WmiObject -Class Win32_Product | Where-Object { $_.Name -match "$AppName" }
if ($App) {
    $App.Uninstall()
    Write-Host "$AppName has been uninstalled."
} else {
    Write-Host "$AppName not found."
}

# Take ownership and reset permissions on folders
$folders = @(
    "C:\ProgramData\Qualys",
    "C:\ProgramFiles\Qualys"
)

foreach ($folder in $folders) {
    if (Test-Path $folder) {
        takeown /F $folder /R /A
        icacls $folder /T /Q /C /RESET
        Remove-Item -Path $folder -Recurse -Force
        Write-Host "Deleted folder $folder."
    } else {
        Write-Host "Folder $folder not found."
    }
}

# Remove Registry keys
$registryPaths = @(
    "HKLM:\SYSTEM\CurrentControlSet\services\QualysAgent",
    "HKLM:\SYSTEM\ControlSet001\Services\QualysAgent",
    "HKLM:\SYSTEM\ControlSet002\Services\QualysAgent",
    "HKLM:\SOFTWARE\Qualys",
    "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Installer\UserData\S-1-5-18\Products\B0276D68D7BC3C348BC53A5177FFA6CE",
    "HKLM:\SOFTWARE\Classes\Installer\Products\B0276D68D7BC3C348BC53A5177FFA6CE"
)

foreach ($path in $registryPaths) {
    if (Test-Path $path) {
        Remove-Item -Path $path -Recurse -Force
        Write-Host "Deleted registry key $path."
    } else {
        Write-Host "Registry key $path not found."
    }
}

# Display message indicating that all traces have been removed from the target machine
Write-Host "All traces of Qualys Agent have been removed from the target machine."
```
