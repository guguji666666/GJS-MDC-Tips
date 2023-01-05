# This article shows the steps to install ASA manually
https://learn.microsoft.com/en-us/azure/defender-for-cloud/auto-deploy-azure-monitoring-agent#additional-extensions-for-defender-for-cloud
```
The Azure Monitor Agent requires additional extensions. 
The ASA extension, which supports endpoint protection recommendations, fileless attack detection, and Adaptive Application controls, is automatically installed when you auto-provision the Azure Monitor Agent.
```

### Install ASA on Azure VM running Windows
#### 1. Get postman
Download postman from https://www.postman.com/downloads/ and launch it.
#### 2. Get user token via https://github.com/guguji666666/GJS-MDC-Tips/blob/main/API/README.md

#### 3. Insert the user token here in postman
![image](https://user-images.githubusercontent.com/96930989/210289242-15003c92-1406-4289-9cfd-a08e5cd7260f.png)

#### 4. Set the `request URL`, `Body` following:
![image](https://user-images.githubusercontent.com/96930989/210707768-4979d7d8-4a3e-4b8d-821e-3234f2704be5.png)

`Binding`: PUT

`URL`
```
https://management.azure.com/subscriptions/<subdid>/resourceGroups/<rgname>/providers/Microsoft.Compute/virtualMachines/<vmname>/extensions/AzureSecurityWindowsAgent?api-version=2019-03-01
```

`Body`
```
{
  "name":"AzureSecurityWindowsAgent", 
  "type":"Microsoft.Compute/virtualmachines/extensions", 
  "location":"<vmlocation>", 
  "properties":{ 
    "autoUpgradeMinorVersion":true, 
    "publisher":"Microsoft.Azure.Security.Monitoring", 
    "type":"AzureSecurityWindowsAgent", 
    "typeHandlerVersion":"1.0",
    "settings":{ },
    "protectedsettings": {}
}
}
```

#### 5. Send the request

