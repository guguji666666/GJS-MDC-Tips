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

## Enable defender for server using `Az powershell`
```powershell
az rest --method put --url https://management.azure.com/subscriptions/<subscription id>/providers/Microsoft.Security/pricings/VirtualMachines?api-version=2023-01-01 --body @<path to your request body json> --headers 'Content-Type=application/json'
```
Sample 
```powershell
az rest --method put --url https://management.azure.com/subscriptions/<subscription id>/providers/Microsoft.Security/pricings/VirtualMachines?api-version=2023-01-01 --body @c:\temp\D4ServerFIM.json --headers 'Content-Type=application/json'
```


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
### We support different types of database in defender for SQL, ARM template would be quicker to enable them all [ARM template to enable defender for SQL](https://github.com/guguji666666/GJS-MDC-Tips/tree/main/Price%20tier/ARM%20Templates/Defender%20for%20SQL)

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
![image](https://github.com/user-attachments/assets/b02de4de-58bb-4958-a02e-a822981d0456)

![image](https://github.com/user-attachments/assets/fb82d860-b504-452c-b787-ef541ebb5588)


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
![image](https://github.com/user-attachments/assets/15829c46-7f4c-4b2b-bbf2-e7bb982fc1ff)

![image](https://github.com/user-attachments/assets/2d453679-006f-4c20-9252-6c427d60ed1a)


## 7.Defender for Resource Manager
### URL
```
https://management.azure.com/subscriptions/<subscription id>/providers/Microsoft.Security/pricings/Arm?api-version=2023-01-01
```
### Difference between legacy and new plan
![image](https://github.com/user-attachments/assets/b2d33489-4409-4d19-ba13-c3f62e544aed)

### Request body (Legacy Per-API-call mode)
```json
{
    "properties": {
        //Legacy plan
        "pricingTier": "Standard",
        "subPlan": "PerApiCall"
    }
}
```
![image](https://github.com/user-attachments/assets/bdf92855-3043-4acd-851b-76b576660c30)


### Request body (New Per-subscription plan)
```json
{
    "properties": {
        //new plan
        "pricingTier": "Standard",
        "subPlan": "PerSubscription"
    }
}
```
![image](https://github.com/user-attachments/assets/744f85df-15fb-4d68-a687-297ffa3c3365)


## 8.Defender for API
### URL
```
https://management.azure.com/subscriptions/<subscription id>/providers/Microsoft.Security/pricings/Api?api-version=2023-01-01
```
###
```json
{
    "properties": {
        "pricingTier": "Standard",
        "subPlan": "P1" // you can modify from P1 to P5
    }
}
```




