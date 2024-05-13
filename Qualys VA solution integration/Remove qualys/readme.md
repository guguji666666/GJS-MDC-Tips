# Uninstall qualys from VM

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
