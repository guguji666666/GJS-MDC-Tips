## Azure VM running Linux TSG steps
#### 1. Check the installation of the qualys extension
![image](https://user-images.githubusercontent.com/96930989/212520057-bd6a74e7-319e-4d40-97a7-8b542bd3c2ac.png)

#### 2. Test connection to qualys cloud service
[What prerequisites and permissions are required to install the Qualys extension?](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-vm#what-prerequisites-and-permissions-are-required-to-install-the-qualys-extension)

[Locations of qualys cloud service(scroll down)](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-vm#deploy-the-integrated-scanner-to-your-azure-and-hybrid-machines)
* https://qagpublic.qg3.apps.qualys.com (64.39.104.113)- Qualys' US data center
* https://qagpublic.qg2.apps.qualys.eu (154.59.121.74)- Qualys' European data center

If your machine is in a region in an Azure `European geography` (such as Europe, UK, Germany), its artifacts will be processed in Qualys' `European data center`.
Artifacts for virtual machines located `elsewhere` are sent to the `US data center`.

Test connection using the command below
```sh
bash -c "</dev/tcp/64.39.104.113/443" && printf "I can reach Movere" || printf "Failure"
```
or
```sh
bash -c "</dev/tcp/154.59.121.74/443" && printf "I can reach Movere" || printf "Failure"
```
Sample
![image](https://user-images.githubusercontent.com/96930989/212526343-36a31837-7f75-421f-a156-7b9fc4a9d94b.png)

#### 3. Check if the metadata is generated
Navigate to the path below and see if `SnapshotVM.db` exists under the path
```
/usr/local/qualys/cloud-agent/SnapshotVM.db
```
![image](https://user-images.githubusercontent.com/96930989/212463514-a666a0cd-8b79-448c-ae3e-27ae47d67960.png)

#### 5. Perform on demand scan, then wait for 24 hours
[Trigger an on-demand scan](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-vm#trigger-an-on-demand-scan)
```sh
sudo /usr/local/qualys/cloud-agent/bin/cloudagentctl.sh action=demand type=vm
```
#### 6. If the issue still persists, reinstall the qualys agent, then wait for 24 hours
1. Romove the qualys extension from Azure VM in Azure portal
2. Run command
```sh
 rpm -e –nodeps "qualys-cloud-agent-x.x.exe"
 ```
3. Remove all the folder named `qualys` under path
```sh
ls /etc
```
4. Reinstall Qualys agent with the options mentioned in [Automate at-scale deployments](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-vm#automate-at-scale-deployments)

#### 7. Collect the logs and reach Microsoft support
Log 1 
```
/var/log/qualys/
```

Log 2
```
/var/log/waagent.log
```
![image](https://user-images.githubusercontent.com/96930989/212526500-19897544-b9f0-405a-865d-b71712cce4fc.png)

Log 3
```
/var/log/azure/Qualys.LinuxAgent.AzureSecurityCenter
```
