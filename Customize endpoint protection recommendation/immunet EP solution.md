## Customize Endpoint Protection Recommendation in MS Defender for Cloud (immunet)
## Steps for deployment
### Before we start
* Please prepare a client machine running win 10/11, install the 3rd party antivirus product (you want to monitor) on it
* We will use old powershell modules since some commands in the script are not supported by the latest powershell modules
* The customed initiative should be assigned to the VMs installed with 3rd party antivirus products `not supported` by Microsoft
* We should avoid duplicated assignment of native endpoint protection recommendation and customed initiative we creat
* It takes hours to update the results in the policies and defender for cloud recommendations

#### 1. Deploy guest configuration extension and enable managed identity on the VM
To audit settings inside a machine, the `Guest Configuration extension` and a `managed identity` are required to audit Azure virtual machines. 

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

For test purpose, we install [Immunet](https://www.immunet.com/index) on the client machine running win10.

![image](https://user-images.githubusercontent.com/96930989/222013844-a562fd62-8fd6-462e-999c-a500372832be.png)

![image](https://user-images.githubusercontent.com/96930989/222013854-307f31bf-e9a3-4027-a32b-3a3620e66104.png)

After installtion, we run the command again to get the `product name` and the `process name`
![image](https://user-images.githubusercontent.com/96930989/221882291-1e347d41-ea78-495c-83cb-5f905377c974.png)

As we can see, the process of immunet is `cscm.exe`, then we ran the command
```cmd
Get-process cscm
```
![image](https://user-images.githubusercontent.com/96930989/221882483-9ce99b16-9249-472a-80f2-f97a9adc49f2.png)

The DSC is using Get-Process that will find the process running in the machine. If there’s no instance of a process running the Antivirus executable found, the validation will stop and returns a status of Stopped providing you a clear description.

Then, we navigate to the path below and modify the configuration file according to your actual environment (the steps are mentioned in the doc [Customizing Endpoint Protection Recommendation in Microsoft Defender for Cloud](https://techcommunity.microsoft.com/t5/microsoft-defender-for-cloud/customizing-endpoint-protection-recommendation-in-microsoft/ba-p/1733217) )
```
C:\Program Files\WindowsPowerShell\Modules\EndPointProtectionDSC\1.0.0.0\AzureGuestConfigurationPolicy\Configurations\MonitorAntivirus.ps1
```

##### Default configuration in the file
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

##### Customize configuration(below is sample)
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
            AntivirusName = "Immunet"
            Status        = "Running"
            Ensure        = "Present"
        }
    }
}
cd $env:Temp
MonitorAntivirus
```

Then navigate to another path to modify the configuration file
```
C:\Program Files\WindowsPowerShell\Modules\EndPointProtectionDSC\1.0.0.0\AzureGuestConfigurationPolicy\ParameterFiles\EPAntivirusStatus.Params.psd1
```
##### Default configuration in the file
```conf
@(
    @{
        Name                 = 'AntivirusName'
        DisplayName          = 'Antivirus Name'
        Description          = "Name of the Antivirus Software to monitor."
        ResourceType         = "EPAntivirusStatus"
        ResourceId           = 'AV'
        ResourcePropertyName = "AntivirusName"
        DefaultValue         = 'Windows Defender'
        #AllowedValues        = @('Avast','Windows Defender','CrowdStrike','Sentinel One')
    }
)
```

##### Customize configuration
```conf
@(
    @{
        Name                 = 'AntivirusName'
        DisplayName          = 'Antivirus Name'
        Description          = "Name of the Antivirus Software to monitor."
        ResourceType         = "EPAntivirusStatus"
        ResourceId           = 'AV'
        ResourcePropertyName = "AntivirusName"
        DefaultValue         = 'Windows Defender'
        AllowedValues        = @('Avast','Windows Defender','CrowdStrike','Sentinel One','Huorong Internet Security','Immunet')
    }
)
```

Once the configuration files are all modified, `Close the powershell client app`, re-launch powershell client app with local admin, run the command:
```powershell
New-EPDSCAzureGuestConfigurationPolicyPackage
```
Then follow the instructions in the script.

Create the resource group as well as the storage account (you can also use the existing the resource group and storage account)
![image](https://user-images.githubusercontent.com/96930989/221900969-531fa125-aeba-4918-9580-e5616e4bd16e.png)


Input the number of the subscription you want to access

![image](https://user-images.githubusercontent.com/96930989/219845118-75d9f7c8-4374-4c0b-8a8b-b647f2d9de96.png)


Wait until the policies and initiative are created in your subscription
![image](https://user-images.githubusercontent.com/96930989/219845150-cb7b9cdf-560b-4f6d-983d-afb23c245de6.png)

![image](https://user-images.githubusercontent.com/96930989/219845124-653be203-da7b-494d-9aea-528f698a178a.png)

To verify in the portal, navigate to `Policy > Definitions`, you will find the new initiative `[Initiative] Monitor Antivirus` here
![image](https://user-images.githubusercontent.com/96930989/219845340-11873fc6-8b3a-4a8d-b903-c035534cc3ae.png)


#### 4. Assign the initiative to the subscription (you can exclude the resource groups where the VMs are using endpoint products supported by Microsoft)

* Edit initiative definition (select the value of the 3rd party endpoint protection product you want to monitor) and save it
![image](https://user-images.githubusercontent.com/96930989/221903000-f082658e-5cae-4013-8329-5895c92e8fa6.png)

* Assign the initiave to the subscription level and create the exemption if required
![image](https://user-images.githubusercontent.com/96930989/219846522-73e84938-6496-46bd-9497-32152d461c5c.png)

* Edit parameter when create the assignment (select the name of the 3rd party antivirus product you want to monitor)
![image](https://user-images.githubusercontent.com/96930989/221903352-7a23884b-f667-4250-bd7f-19ce49e3f8d6.png)

* Create remediation task so that the initiative will apply to the existing VMs
![image](https://user-images.githubusercontent.com/96930989/221903629-711b59cf-37d6-4af0-b4d8-2c98ca27028a.png)

* Refer to the doc [Add a custom initiative to your subscription](https://learn.microsoft.com/en-us/azure/defender-for-cloud/custom-security-policies?pivots=azure-portal#to-add-a-custom-initiative-to-your-subscription)

* Add the initiative to the subscription level or above
![image](https://user-images.githubusercontent.com/96930989/219845590-1d7ee33e-ddd6-49b0-b6a5-9c59650ea908.png)

Sample
![image](https://user-images.githubusercontent.com/96930989/219846164-91b92d1f-5973-4724-9ca8-575dfc74517b.png)
![image](https://user-images.githubusercontent.com/96930989/219846168-801c8465-8793-4961-a91b-775b03ebd1ea.png)
![image](https://user-images.githubusercontent.com/96930989/219846173-d81f1441-60d0-4a00-b8e7-c40d0546caaa.png)
![image](https://user-images.githubusercontent.com/96930989/219846182-171016b0-5c67-4383-88ff-e79459447910.png)

#### 5. Run the command in Azure cloud shell to reduce the time of policy evaluation
```powershell
$job = Start-AzPolicyComplianceScan -AsJob
$job
```
![image](https://user-images.githubusercontent.com/96930989/219985951-0eabfc52-59b3-4dda-98a1-4117331c6020.png)


#### 6. Wait for 12-24 hours and check the compliance state in new policies (keep VM on)

Navigate to `Policy > Compliance`
![image](https://user-images.githubusercontent.com/96930989/222013357-da9e78d7-f755-4cb7-a41d-61cd00b39d51.png)

Check the compliance state of this initiative
![image](https://user-images.githubusercontent.com/96930989/222013278-489d1eec-8cb9-4118-b8a6-2427366dd5de.png)

Click the `[Audit]Monitor Antivirus` policy
![image](https://user-images.githubusercontent.com/96930989/222013581-cd75db1b-8ab9-4971-9fe0-623e4f77cea3.png)

The client machine we deployed before is showing healthy state
![image](https://user-images.githubusercontent.com/96930989/222013663-477620e4-1d33-4627-b13e-ae836860c96e.png)


#### 7. Check the regulatory compliance in Defender for cloud, wait for 12 hours if the results are not updated

![image](https://user-images.githubusercontent.com/96930989/222014672-47d5e176-943e-4cdb-89af-07f47bd9eaba.png)

We can also check more details in the recommendation
![image](https://user-images.githubusercontent.com/96930989/222014765-2ed8f834-afd1-48e7-a0cc-dac588e75c0c.png)

According to the test results in our lab, the customized initiaitve supports server 2016/2019, win 10/11

