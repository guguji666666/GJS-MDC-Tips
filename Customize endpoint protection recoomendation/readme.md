## Customize Endpoint Protection Recommendation in Microsoft Defender for Cloud
##### Microsoft Defender for Cloud covers a variety of Antimalware vendors today. You can find the list of supported versions in [Supported endpoint protection solutions](https://learn.microsoft.com/en-us/azure/defender-for-cloud/supported-machines-endpoint-solutions-clouds-servers?tabs=features-windows#supported-endpoint-protection-solutions)
![image](https://user-images.githubusercontent.com/96930989/219843241-37347dd4-510c-4dae-a531-4da58dc993a0.png)

However, in a scenario where you’re using an antimalware protection that is not in the Microsoft Defender for Cloud supported list but you still want to have visibility over the status of this antimalware using Defender for Cloud dashboard,Microsoft Defender for Cloud covers a variety of Antimalware vendors today. You can find the list of supported versions in this article. However, in a scenario where you’re using an antimalware protection that is not in the Microsoft Defender for Cloud supported list but you still want to have visibility over the status of this antimalware using Defender for Cloud dashboard,.
The the steps mentioned below will be a workaround.

### Steps for deployment https://techcommunity.microsoft.com/t5/microsoft-defender-for-cloud/customizing-endpoint-protection-recommendation-in-microsoft/ba-p/1733217
#### 1. Deploy guest configuration extension and enable managed identity on the VM
To audit settings inside a machine, the Guest Configuration extension and a managed identity is required to audit Azure virtual machines. 

To deploy the extension at scale, make sure to choose appropriate scope and assign the policy initiative `Deploy prerequisites to enable Guest Configuration policies on virtual machines` under Azure Policies as shown in the image below.
![image](https://user-images.githubusercontent.com/96930989/219843355-37f9d15a-0f1b-48ef-93ab-9b3f96c7e4e5.png)

Check what's inside the policies
![image](https://user-images.githubusercontent.com/96930989/219843366-5ee14294-b364-48c6-b5a2-ab7c2c67b449.png)

Check the scope during assignment
![image](https://user-images.githubusercontent.com/96930989/219843387-793aaa1d-ff67-4ab2-95c7-384febacec93.png)

Deploy `remediation task` so that the guest configuration extension could be deployed on `existing VMs` as well (not only new VMs created in the future)
![image](https://user-images.githubusercontent.com/96930989/219843400-99ceb4aa-2252-451c-b6e1-b0a7d1d4a7b0.png)

Navigate to `Policy>Remediation`, you can check the number of existing VMs applied with remediation task
![image](https://user-images.githubusercontent.com/96930989/219844128-a81c874b-cbbe-4141-b54e-956102ad17ab.png)

You can also check the status of the remediation task
![image](https://user-images.githubusercontent.com/96930989/219844136-1130a435-6cf2-47e6-8cf6-c1b5e7dc4420.png)

![image](https://user-images.githubusercontent.com/96930989/219844144-2166fea9-c648-49c5-bde9-fb49cff5fce0.png)

Once the remediation taks has completed, we can check the `guest configuration extension` on the VM
![image](https://user-images.githubusercontent.com/96930989/219844174-e9636464-e9ac-4a91-ab03-bc4687951edc.png)

The `system assigned identity` should also been enabled on the VM at that point
![image](https://user-images.githubusercontent.com/96930989/219844190-1660fd0b-7352-499f-9a07-6114f26bd74e.png)


#### 2. Install the powershell modules required (on win10/11 client machine)
* [EndPointProtectionDSC powershell module](https://www.powershellgallery.com/packages/GuestConfiguration/3.0.0)
* [GuestConfiguration powershell module](https://www.powershellgallery.com/packages/EndPointProtectionDSC/1.0.0.0)
```powershell
Set-ExecutionPolicy RemoteSigned
  
Install-Module Az -Force
    
Install-Module -Name EndPointProtectionDSC -Force
  
Import-module -Name EndPointProtectionDSC

Install-Module -Name GuestConfiguration -RequiredVersion 1.19.4 -Force

Import-module -Name GuestConfiguration
```
Download the powershell module from [Customed EndPointProtectionDSC module](https://github.com/Azure/Microsoft-Defender-for-Cloud/tree/main/Remediation%20scripts/Customize%20Endpoint%20Protection%20Recommendation) using the tool [GitZip](https://kinolien.github.io/gitzip/)

Input the url of the github file you want to download
![image](https://user-images.githubusercontent.com/96930989/219844419-5501096e-4d91-4cbe-acc1-5d53593bb704.png)

You will then get the files below
![image](https://user-images.githubusercontent.com/96930989/219844449-fda72f31-0243-49c2-a4ed-b695e3b7aad0.png)


Unzip the file and you will see
![image](https://user-images.githubusercontent.com/96930989/219844457-028a3e5b-06b7-4f1a-9402-57cc1f810d55.png)
![image](https://user-images.githubusercontent.com/96930989/219844458-650acd37-c5cb-4e30-87a7-ddc6c3b5777d.png)

Then navigate to the path `C:\Program Files\WindowsPowerShell\Modules`, we can find the files below (generated when we install the module EndPointProtectionDSC)
```
C:\Program Files\WindowsPowerShell\Modules
```
![image](https://user-images.githubusercontent.com/96930989/219844502-56c131aa-112c-43ed-b9de-0953b41cad25.png)

Replace the folders `AzureGuestConfigurationPolicy` and `DSCResources` we downloaded from github 
![image](https://user-images.githubusercontent.com/96930989/219844524-b8d6b618-8d08-42c5-a525-95041e9c01b5.png)



Navigate to the path `C:\Program Files\WindowsPowerShell\Modules\EndPointProtectionDSC\1.0.0.0\AzureGuestConfigurationPolicy\AzureGuestPolicyHelper.psm1`

Search for `New-GuestConfigurationPackage`

Delete `-Force` behind `"$env:Temp/MonitorAntivirus/MonitorAntivirus.mof"` , save the file
```
C:\Program Files\WindowsPowerShell\Modules\EndPointProtectionDSC\1.0.0.0\AzureGuestConfigurationPolicy\AzureGuestPolicyHelper.psm1
```

```
New-GuestConfigurationPackage
```

Before

![image](https://user-images.githubusercontent.com/96930989/219844565-9b122a22-9f2d-45d8-a476-cb193d5333ee.png)

After

![image](https://user-images.githubusercontent.com/96930989/219844575-be4b9602-70df-4ef2-8e79-faf651151709.png)


#### 3. Check the process name of the 3rd party endpoint protection product you want o add
Command to check the current antivirus products on the machine
```cmd
Get-CimInstance -Namespace "root\securitycenter2" -ClassName AntivirusProduct
```
![image](https://user-images.githubusercontent.com/96930989/219844642-a392c40f-95f5-45bd-a120-2b2170f47a03.png)

For test purpose, we install [Huorong Internet security](https://www.huorong.cn/) on the client machine running win10.
After installtion, we run the command again:
![image](https://user-images.githubusercontent.com/96930989/219844742-3a785232-3c52-4245-aae0-8732089e7c51.png)


As we can see, the process of huorong is `wsctrlsvc.exe`, then we ran the command
```cmd
Get-process wsctrlsvc
```
![image](https://user-images.githubusercontent.com/96930989/219844775-4213101f-0927-4bf7-973e-c56c8388d569.png)

The DSC is using Get-Process that will find the process running in the machine. If there’s no instance of a process running the Antivirus executable found, the validation will stop and returns a status of Stopped providing you a clear description.

Then, we navigate to the path below and modify the file according to your actual environment
```
C:\Program Files\WindowsPowerShell\Modules\EndPointProtectionDSC\1.0.0.0\AzureGuestConfigurationPolicy\Configurations\MonitorAntivirus.ps1
```

##### Default configuration
```conf
Configuration MonitorAntivirus
{
    Import-DscResource -ModuleName EndPointProtectionDSC
    Node MonitorAntivirus
    {
        EPAntivirusStatus AV
        {
            AntivirusName = "Windows Defender"
            Status        = "Running"
            Ensure        = "Present"
        }
    }
}
cd $env:Temp
MonitorAntivirus
```

##### Custom configuration(below is sample)
```conf
Configuration MonitorAntivirus
{
    Import-DscResource -ModuleName EndPointProtectionDSC
    Node MonitorAntivirus
    {
        EPAntivirusStatus AV
        {
            AntivirusName = "Windows Defender"
            Status        = "Running"
            Ensure        = "Present"
        };
        {
            AntivirusName = "Huorong Internet Security"
            Status        = "Running"
            Ensure        = "Present"
        }
    }
}
cd $env:Temp
MonitorAntivirus
```

s
