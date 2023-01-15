## JIT TSG steps
### Lab for insufficient JIT(requesting) permissions
#### 1. We create a user and only assign the `read` permission on the VM (Microsoft.Compute/virtualMachines/read)
![image](https://user-images.githubusercontent.com/96930989/212519542-60efd0a3-76b7-4fef-b0e6-83d86564a447.png)
#### 2. Check the behavior when user tries to open the `Networking` and `Connect` page

Networking
![image](https://user-images.githubusercontent.com/96930989/212519656-0f521b7a-1195-48fc-b3eb-a7631acf6446.png)

Connect
![image](https://user-images.githubusercontent.com/96930989/212519676-765255fb-0a85-4339-ab6f-597811f93115.png)

Capture HAR following [Steps](https://github.com/guguji666666/Logs-tracing/tree/main/HAR)

![image](https://user-images.githubusercontent.com/96930989/212519779-67fff511-e693-4dec-bdb1-ca5bcd709e9b.png)

![image](https://user-images.githubusercontent.com/96930989/212519869-4681be02-fbf7-4b40-b1cc-e00e69d874df.png)

