## Guest configuration 

### 1. Steps to reinstall guest configuration extension

#### 1. Uninstall guest configuration from Azure VM
![image](https://user-images.githubusercontent.com/96930989/233086469-31dcaf22-3cd0-4af5-bb43-8ef785a78ec1.png)

#### 2. Add Service tags  to port 80 and 443
* AzureArcInfrastructure
* Storage
* GuestAndHybridManagement

Sample (`Service tag` AzureArcInfrastructure, `Port`80) <br>
<img width="1929" alt="image" src="https://user-images.githubusercontent.com/96930989/233061489-abb59dd6-4ee1-45b7-b2a0-dad24b980fd4.png">

#### 3. Enable System assigned identity 

##### Remove files from local machine

C:\ProgramData  <br>
<img width="814" alt="image" src="https://user-images.githubusercontent.com/96930989/233085903-dd13f0b4-f2d9-4d0d-9576-98b8a985a2ba.png">

C:\Packages\Plugins  <br>
<img width="936" alt="image" src="https://user-images.githubusercontent.com/96930989/233087257-af642ef1-fa85-4a2e-bc3e-5bb8990c0834.png">

C:\WindowsAzure\Logs\Plugins  <br>
<img width="981" alt="image" src="https://user-images.githubusercontent.com/96930989/233087464-de4c3737-30d7-46a6-91db-8b271054d2ba.png">

#### 5. Reboot the machine
#### 6. Run Azure cloudshell command to trigger manual scan
```powershell
$job = Start-AzPolicyComplianceScan -AsJob
```

### 2. Check reports in guest assignments UI

#### 1. Search `Guest assignments` in Azure portal
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/4140907e-d59c-45d4-9a94-5bdc16a665ea)

Here you can see the overview of the guest assignments <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/f6794782-37dd-4304-9b1d-ae6132ba961a)

You can also set the filter for `Subscription`, `Virtual machine` to get the results of specified VM <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/1cbfa251-c210-4f1d-aeff-9dc014734925)


### 3. Check the findings for specified VM in recommendation 
#### Sample
Vulnerabilities in security configuration on your Linux machines should be remediated (powered by Guest Configuration) <br>
Click `Affeceted resources` > `Unhealthy resources`, search for the VM you want to investigate <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/a5806778-4728-43a3-8a2b-194826d3e51f)

Then you will see the findings from this VM <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/5b0e48f3-83f5-49ba-8598-626d8cffc1d6)




