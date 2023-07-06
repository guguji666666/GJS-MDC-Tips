## Manage Arc VM extensions

[Enable Azure VM extensions using the Azure CLI](https://learn.microsoft.com/en-us/azure/azure-arc/servers/manage-vm-extensions-cli#list-extensions-installed)


## Connect VM to Arc

### 1.AWS VM

VM1 running AWS Linux OS

cat /etc/os-release

![image](https://user-images.githubusercontent.com/96930989/231650534-65aab03e-72d9-45d7-83c4-7e6d2cf3a39b.png)

Try to run the script to connect to Azure Arc but failed

![image](https://user-images.githubusercontent.com/96930989/231650683-b11a68b4-3d0c-4822-aa89-df03bfa875dc.png)


VM2 running AWS Linux OS

cat /etc/os-release

![image](https://user-images.githubusercontent.com/96930989/232210087-7699cb01-fe1a-4258-a251-f639db1d89d7.png)

Connected to Azure Arc succeeds, but failed to install ASA extension

![image](https://user-images.githubusercontent.com/96930989/232210052-3c2a8d83-e812-4ceb-ac64-52f12dbcc073.png)

Error message

```
Extension returned non-zero exit code for Install: 14. Extension error output: [AzureSecurityLinuxAgent] 2023/04/13 07:34:57 INFO: seqnum: 0

[AzureSecurityLinuxAgent] 2023/04/13 07:34:57 ERR: OS is not supported. Error: Invalid system-release-cpe file data

.  Extension Message: Install ASM failed: OS is not supported. Error: Invalid system-release-cpe file data

Extension Error: [AzureSecurityLinuxAgent] 2023/04/13 07:22:01 INFO: seqnum: 0

[AzureSecurityLinuxAgent] 2023/04/13 07:22:01 ERR: OS is not supported. Error: Invalid system-release-cpe file data

[AzureSecurityLinuxAgent] 2023/04/13 07:34:57 INFO: seqnum: 0

[AzureSecurityLinuxAgent] 2023/04/13 07:34:57 ERR: OS is not supported. Error: Invalid system-release-cpe file data
```

### 2. VM running Debian 11

![image](https://user-images.githubusercontent.com/96930989/232307325-a853a1a4-432e-41c5-b0c8-78e9fe621083.png)

Connected to Azure Arc succeeds, but failed to install ASA extension

Error
```
Extension returned non-zero exit code for Install: 14. Extension error output: [AzureSecurityLinuxAgent] 2023/04/15 10:03:15 INFO: seqnum: 0
[AzureSecurityLinuxAgent] 2023/04/15 10:03:15 ERR: OS is not supported. Error: Invalid system-release-cpe file data
.  Extension Message: Install ASM failed: OS is not supported. Error: Invalid system-release-cpe file data
Extension Error: [AzureSecurityLinuxAgent] 2023/04/13 07:22:01 INFO: seqnum: 0
[AzureSecurityLinuxAgent] 2023/04/13 07:22:01 ERR: OS is not supported. Error: Invalid system-release-cpe file data
[AzureSecurityLinuxAgent] 2023/04/13 07:34:57 INFO: seqnum: 0
[AzureSecurityLinuxAgent] 2023/04/13 07:34:57 ERR: OS is not supported. Error: Invalid system-release-cpe file data
[AzureSecurityLinuxAgent] 2023/04/14 03:35:36 INFO: seqnum: 0
[AzureSecurityLinuxAgent] 2023/04/14 03:35:36 ERR: OS is not supported. Error: Invalid system-release-cpe file data
[AzureSecurityLinuxAgent] 2023/04/15 10:03:15 INFO: seqnum: 0
[AzureSecurityLinuxAgent] 2023/04/15 10:03:15 ERR: OS is not supported. Error: Invalid system-release-cpe file data
```
