# Enable defender for cloud plan using API

## Method
```
PUT
```

## 1.Defender for server
### URL
```
https://management.azure.com/subscriptions/<subscription id>/providers/Microsoft.Security/pricings/VirtualMachines?api-version=2023-01-01
```

### Request body
```json
{
    "properties": {
        "extensions": [
            {
                "name": "MdeDesignatedSubscription",
                "isEnabled": "False"
            },
            {
                "name": "AgentlessVmScanning",
                "isEnabled": "True",
                "additionalExtensionProperties": {
                    "ExclusionTags": "[]"
                }
            },
            {
                "name": "FileIntegrityMonitoring",
                "isEnabled": "True",
                "additionalExtensionProperties": {
                    "DefinedWorkspaceId": "<resource id of destination log analytics workspace>", //input resource id of the workspace
                    "Rules": "[{\"RuleId\":\"1c18eca2-7d3b-4367-3397-097a1ef2edb1\",\"MonitoredEntities\":{\"WindowsFiles\":[{\"Path\":\"\\\\Windows\\\\system.ini\"},{\"Path\":\"\\\\Windows\\\\win.ini\"},{\"Path\":\"\\\\Windows\\\\regedit.exe\"},{\"Path\":\"\\\\Windows\\\\explorer.exe\"},{\"Path\":\"\\\\Windows\\\\System32\\\\userinit.exe\"},{\"Path\":\"autoexec.bat\"},{\"Path\":\"boot.ini\"},{\"Path\":\"config.sys\"}],\"LinuxFiles\":[{\"Path\":\"/bin/login\"},{\"Path\":\"/bin/passwd\"},{\"Path\":\"/etc/crontab\"},{\"Path\":\"/etc/*.conf\"},{\"Path\":\"/usr/bin/\"},{\"Path\":\"/usr/sbin/\"},{\"Path\":\"/bin/\"},{\"Path\":\"/sbin/\"},{\"Path\":\"/boot/\"},{\"Path\":\"/usr/local/bin/\"},{\"Path\":\"/usr/local/sbin/\"},{\"Path\":\"/opt/bin/\"},{\"Path\":\"/opt/sbin/\"},{\"Path\":\"/etc/init.d/\"},{\"Path\":\"/etc/cron.hourly/\"},{\"Path\":\"/etc/cron.daily/\"},{\"Path\":\"/etc/cron.weekly/\"},{\"Path\":\"/etc/cron.monthly/\"}],\"Registries\":[{\"Path\":\"hklm\\\\SOFTWARE\\\\Microsoft\\\\Windows NT\\\\CurrentVersion\\\\Windows\\\\loadappinit_dlls\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\Microsoft\\\\Windows NT\\\\CurrentVersion\\\\Windows\\\\appinit_dlls\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\Microsoft\\\\Windows NT\\\\CurrentVersion\\\\Windows\\\\iconservicelib\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\Microsoft\\\\Windows\\\\CurrentVersion\\\\Explorer\\\\Shell Folders\\\\common startup\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\Microsoft\\\\Windows\\\\CurrentVersion\\\\Explorer\\\\Shell Folders\\\\startup\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\Microsoft\\\\Windows\\\\CurrentVersion\\\\Explorer\\\\User Shell Folders\\\\common startup\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\Microsoft\\\\Windows\\\\CurrentVersion\\\\Explorer\\\\User Shell Folders\\\\startup\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\Microsoft\\\\Windows\\\\CurrentVersion\\\\Run\\\\\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\Microsoft\\\\Windows\\\\CurrentVersion\\\\RunOnce\\\\\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\Microsoft\\\\Windows\\\\CurrentVersion\\\\RunServicesOnce\\\\\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\WOW6432Node\\\\Microsoft\\\\Windows NT\\\\CurrentVersion\\\\Windows\\\\appinit_dlls\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\WOW6432Node\\\\Microsoft\\\\Windows NT\\\\CurrentVersion\\\\Windows\\\\loadappinit_dlls\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\WOW6432Node\\\\Microsoft\\\\Windows\\\\CurrentVersion\\\\Explorer\\\\Shell Folders\\\\common startup\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\WOW6432Node\\\\Microsoft\\\\Windows\\\\CurrentVersion\\\\Explorer\\\\Shell Folders\\\\startup\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\WOW6432Node\\\\Microsoft\\\\Windows\\\\CurrentVersion\\\\Explorer\\\\User Shell Folders\\\\common startup\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\WOW6432Node\\\\Microsoft\\\\Windows\\\\CurrentVersion\\\\Explorer\\\\User Shell Folders\\\\startup\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\WOW6432Node\\\\Microsoft\\\\Windows\\\\CurrentVersion\\\\Run\\\\\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\WOW6432Node\\\\Microsoft\\\\Windows\\\\CurrentVersion\\\\RunOnce\\\\\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\WOW6432Node\\\\Microsoft\\\\Cryptography\\\\OID\\\\\"},{\"Path\":\"hklm\\\\SOFTWARE\\\\Microsoft\\\\Cryptography\\\\OID\\\\\"},{\"Path\":\"hklm\\\\SECURITY\\\\POLICY\\\\SECRETS\\\\\"}]}}]"
                }
            }
        ],
        "subPlan": "P2",
        "pricingTier": "Standard"
    }
}
```
Sample in Bruno <br>
![image](https://github.com/user-attachments/assets/e71b797b-504a-4c0f-b85a-10b5f72c2549)

Using GET for validation <br>
![image](https://github.com/user-attachments/assets/9f69433f-4952-48f3-bf1b-80497325281a)

## 2.Defender for APP Service
### URL
```
https://management.azure.com/subscriptions/<subscription id>/providers/Microsoft.Security/pricings/AppServices?api-version=2023-01-01
```
### Request body
```json
{
    "properties": {
        "pricingTier": "Standard"
    }
}
```
![image](https://github.com/user-attachments/assets/00ff45f6-de4e-4ea0-a28a-cf9eb1bc8975)

## 3.Defender for SQL
### URL
```
https://management.azure.com/subscriptions/<subscription id>/providers/Microsoft.Security/pricings/VirtualMachines?api-version=2023-01-01
```
## 4.Defender for Storage
### URL
```
https://management.azure.com/subscriptions/<subscription id>/providers/Microsoft.Security/pricings/StorageAccounts?api-version=2023-01-01
```
### Request body
```json
{
    "properties": {
        "extensions": [
            {
                "name": "OnUploadMalwareScanning",
                "isEnabled": "True",
                "additionalExtensionProperties": {
                    "CapGBPerMonthPerStorageAccount": "8000" //You can customize the monthly cap here
                }
            },
            {
                "name": "SensitiveDataDiscovery",
                "isEnabled": "True"
            }
        ],
        "pricingTier": "Standard",
        "subPlan": "DefenderForStorageV2"
    }
}
```
![image](https://github.com/user-attachments/assets/c7932e56-e0b2-4036-b463-7e52c71ea50d)


## 5.Defender for Container
### URL
```
https://management.azure.com/subscriptions/<subscription id>/providers/Microsoft.Security/pricings/Containers?api-version=2023-01-01
```

### Request body
```json
{
    "properties": {
        "extensions": [
            {
                "name": "ContainerRegistriesVulnerabilityAssessments",
                "isEnabled": "True"
            },
            // {
            //     "name": "ContainerIntegrityContribution",
            //     "isEnabled": "True"
            // },
            {
                "name": "AgentlessDiscoveryForKubernetes",
                "isEnabled": "True"
            },
            {
                "name": "AgentlessVmScanning",
                "isEnabled": "True",
                "additionalExtensionProperties": {
                    "ExclusionTags": "[]"
                }
            },
            {
                "name": "ContainerSensor",
                "isEnabled": "True"
            }
        ],
        "pricingTier": "Standard"
    }
}
```
![image](https://github.com/user-attachments/assets/6202943c-a390-47c3-8121-3d26be890de8)


## 6.Defender for Key Vault
### URL
```
https://management.azure.com/subscriptions/<subscription id>/providers/Microsoft.Security/pricings/KeyVaults?api-version=2023-01-01
```
### Difference between legacy and new plan
![image](https://github.com/user-attachments/assets/43825466-f3a6-4259-b6b8-030fecadc14b)

### Request body （legacy plan)
```json
{
    "properties": {
        //Legacy plan
        "pricingTier": "Standard",
        "subPlan": "PerTransaction"
    }
}
```

### Request body （new plan)
```json
{
    "properties": {
        //new plan
        "pricingTier": "Standard",
        "subPlan": "PerKeyVault"
    }
}
```

## 7.Defender for Resource Manager
### URL
```
https://management.azure.com/subscriptions/<subscription id>/providers/Microsoft.Security/pricings/VirtualMachines?api-version=2023-01-01
```
## 8.Defender for API
### URL
```
https://management.azure.com/subscriptions/<subscription id>/providers/Microsoft.Security/pricings/VirtualMachines?api-version=2023-01-01
```





