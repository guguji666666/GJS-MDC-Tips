## Steps to deploy BYOL VA solution
[Deploy a bring your own license (BYOL) vulnerability assessment solution](https://learn.microsoft.com/en-us/azure/defender-for-cloud/deploy-vulnerability-assessment-byol-vm)

### Configure a new BYOL solution in your subscription
1. Defender for cloud -> recommendation -> Machines should have a vulnerability assessment solution -> fix
![image](https://user-images.githubusercontent.com/96930989/213363188-c2164819-4b1e-4ac3-8362-c21c0f00025a.png)

2. Click `Configure a new third-party vulnerability scanner (BYOL - requires a separate license)`
![image](https://user-images.githubusercontent.com/96930989/213363249-0c70bfe7-f470-432f-9457-46f7cacbd8c3.png)

3. Fill your license code and public key.
![image](https://user-images.githubusercontent.com/96930989/213363258-3e164de5-cb0a-46f1-95cb-03ccc3d90dc7.png)


### Apply existing BYOL solution to your VM
1. Defender for cloud -> recommendation -> Machines should have a vulnerability assessment solution -> fix
![image](https://user-images.githubusercontent.com/96930989/213402182-bfa3d63a-c330-4304-9000-cb2249601855.png)

2. Click `Deploy your configured third-party vulnerability scanner (BYOL - requires a separate license)`

3. Proceed