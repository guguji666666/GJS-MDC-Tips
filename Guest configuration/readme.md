## Guest configuration 

### 1. Steps to reinstall guest configuration extension

1. Uninstall guest configuration from Azure VM
![image](https://user-images.githubusercontent.com/96930989/233086469-31dcaf22-3cd0-4af5-bb43-8ef785a78ec1.png)

2. Add Service tags  to port 80 and 443
* AzureArcInfrastructure
* Storage
* GuestAndHybridManagement

Sample <br>
<img width="1929" alt="image" src="https://user-images.githubusercontent.com/96930989/233061489-abb59dd6-4ee1-45b7-b2a0-dad24b980fd4.png">

3. Enable System assigned identity 

Remove files from local machine

C:\ProgramData  <br>
<img width="814" alt="image" src="https://user-images.githubusercontent.com/96930989/233085903-dd13f0b4-f2d9-4d0d-9576-98b8a985a2ba.png">

C:\Packages\Plugins  <br>
<img width="936" alt="image" src="https://user-images.githubusercontent.com/96930989/233087257-af642ef1-fa85-4a2e-bc3e-5bb8990c0834.png">

C:\WindowsAzure\Logs\Plugins  <br>
<img width="981" alt="image" src="https://user-images.githubusercontent.com/96930989/233087464-de4c3737-30d7-46a6-91db-8b271054d2ba.png">

5. Reboot the machine

6. Run command for manual scan
```powershell
$job = Start-AzPolicyComplianceScan -AsJob
```
