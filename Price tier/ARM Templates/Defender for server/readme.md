# Defender for server

## ARM template (will encounter error)
```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Security/pricings",
            "apiVersion": "2023-01-01",
            "name": "VirtualMachines",
            "properties": {
                "pricingTier": "Standard",
                "subPlan": "P2",
                "extensions": [
                    {
                        "name": "MdeDesignatedSubscription",
                        "isEnabled": "False"
                    }, // Uncomment if needed
                    {
                        "name": "AgentlessVmScanning",
                        "isEnabled": "True",
                        "additionalExtensionProperties": {
                            "ExclusionTags": "[json('[]')]"
                        }
                    },
                    {
                        "name": "FileIntegrityMonitoring",
                        "isEnabled": "True",
                        "additionalExtensionProperties": {
                            "DefinedWorkspaceId": "<resource id of the workspace>",
                            "Rules": [
                                {
                                    "RuleId": "b0a324d7-9f1a-477c-b955-a84c9b9bfb9f",
                                    "MonitoredEntities": {
                                        "WindowsFiles": [
                                            {"Path": "\\Windows\\system.ini"},
                                            {"Path": "\\Windows\\win.ini"},
                                            {"Path": "\\Windows\\regedit.exe"},
                                            {"Path": "\\Windows\\explorer.exe"},
                                            {"Path": "\\Windows\\System32\\userinit.exe"},
                                            {"Path": "autoexec.bat"},
                                            {"Path": "boot.ini"},
                                            {"Path": "config.sys"}
                                        ],
                                        "LinuxFiles": [
                                            {"Path": "/bin/login"},
                                            {"Path": "/bin/passwd"},
                                            {"Path": "/etc/crontab"},
                                            {"Path": "/etc/*.conf"},
                                            {"Path": "/usr/bin/"},
                                            {"Path": "/usr/sbin/"},
                                            {"Path": "/bin/"},
                                            {"Path": "/sbin/"},
                                            {"Path": "/boot/"},
                                            {"Path": "/usr/local/bin/"},
                                            {"Path": "/usr/local/sbin/"},
                                            {"Path": "/opt/bin/"},
                                            {"Path": "/opt/sbin/"},
                                            {"Path": "/etc/init.d/"},
                                            {"Path": "/etc/cron.hourly/"},
                                            {"Path": "/etc/cron.daily/"},
                                            {"Path": "/etc/cron.weekly/"},
                                            {"Path": "/etc/cron.monthly/"}
                                        ],
                                        "Registries": [
                                            {"Path": "hklm\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Windows\\loadappinit_dlls"},
                                            {"Path": "hklm\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Windows\\appinit_dlls"},
                                            {"Path": "hklm\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Windows\\iconservicelibs"},
                                            {"Path": "hklm\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Explorer\\Shell Folders\\common startup"},
                                            {"Path": "hklm\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Explorer\\Shell Folders\\startup"},
                                            {"Path": "hklm\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Explorer\\User Shell Folders\\common startup"},
                                            {"Path": "hklm\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Explorer\\User Shell Folders\\startup"},
                                            {"Path": "hklm\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Run\\"},
                                            {"Path": "hklm\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\RunOnce\\"},
                                            {"Path": "hklm\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\RunServicesOnce\\"},
                                            {"Path": "hklm\\SOFTWARE\\WOW6432Node\\Microsoft\\Windows NT\\CurrentVersion\\Windows\\appinit_dlls"},
                                            {"Path": "hklm\\SOFTWARE\\WOW6432Node\\Microsoft\\Windows NT\\CurrentVersion\\Windows\\loadappinit_dlls"},
                                            {"Path": "hklm\\SOFTWARE\\WOW6432Node\\Microsoft\\Windows\\CurrentVersion\\Explorer\\Shell Folders\\common startup"},
                                            {"Path": "hklm\\SOFTWARE\\WOW6432Node\\Microsoft\\Windows\\CurrentVersion\\Explorer\\Shell Folders\\startup"},
                                            {"Path": "hklm\\SOFTWARE\\WOW6432Node\\Microsoft\\Windows\\CurrentVersion\\Explorer\\User Shell Folders\\common startup"},
                                            {"Path": "hklm\\SOFTWARE\\WOW6432Node\\Microsoft\\Windows\\CurrentVersion\\Explorer\\User Shell Folders\\startup"},
                                            {"Path": "hklm\\SOFTWARE\\WOW6432Node\\Microsoft\\Windows\\CurrentVersion\\Run\\"},
                                            {"Path": "hklm\\SOFTWARE\\WOW6432Node\\Microsoft\\Windows\\CurrentVersion\\RunOnce\\"},
                                            {"Path": "hklm\\SOFTWARE\\WOW6432Node\\Microsoft\\Cryptography\\OID\\"},
                                            {"Path": "hklm\\SOFTWARE\\Microsoft\\Cryptography\\OID\\"},
                                            {"Path": "hklm\\SECURITY\\POLICY\\SECRETS\\"}
                                        ]
                                    }
                                }
                            ]
                        }
                    }
                ]
            }
        }
    ]
}
```

## API (recommended)
### Method
```
PUT
```

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
                "isEnabled": "False"
            }
        ],
        "subPlan": "P2",
        "pricingTier": "Standard"
    }
}
```
