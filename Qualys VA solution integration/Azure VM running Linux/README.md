## Azure VM running Linux TSG steps
#### 1. Check the installation of the qualys extension
![image](https://user-images.githubusercontent.com/96930989/212520057-bd6a74e7-319e-4d40-97a7-8b542bd3c2ac.png)

#### 2. Test connection to qualys cloud service
#### 3. Check if the metadata is generated
Navigate to the path below and see if `SnapshotVM.db` exists
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
#### 7. Collect the logs and reach Microsoft support
