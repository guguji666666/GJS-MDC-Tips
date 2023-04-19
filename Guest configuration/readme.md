## Guest configuration 

### 1. Steps to reinstall guest configuration extension

1. Uninstall guest configuration from Azure VM

2. Add Service tags  to port 80 and 443
* AzureArcInfrastructure
* Storage
* GuestAndHybridManagement

Sample <br>
<img width="1929" alt="image" src="https://user-images.githubusercontent.com/96930989/233061489-abb59dd6-4ee1-45b7-b2a0-dad24b980fd4.png">

3. Enable System assigned identity 

Remove files from local machine

<img width="1138" alt="image" src="https://user-images.githubusercontent.com/96930989/233060385-0995cecf-d581-4d54-a9c8-6f2e34c1a176.png">

<img width="916" alt="image" src="https://user-images.githubusercontent.com/96930989/233060430-ecba883b-428a-41eb-835c-53e81d9badc6.png">

<img width="1002" alt="image" src="https://user-images.githubusercontent.com/96930989/233060446-66bef3b2-5bbf-4ea2-a4ac-3f3a348cf414.png">

5. Reboot the machine

6. Run command for manual scan
