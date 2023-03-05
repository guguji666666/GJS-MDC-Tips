## Connect AWS to Microsoft defender for cloud

### 1. Create a new [AWS account]((https://www.youtube.com/watch?v=AxsFAuBnQWE))

### 2. [Create VM in AWS](https://www.youtube.com/watch?v=JgfWYcNR6nk)

Navigate to EC2
![image](https://user-images.githubusercontent.com/96930989/222940188-39e7d9f4-e5d8-470b-b909-a172e84cb5f1.png)

Create security group if required
![image](https://user-images.githubusercontent.com/96930989/222941703-f503a1c4-8679-423a-aee2-736ee0d344e9.png)

![image](https://user-images.githubusercontent.com/96930989/222941716-15352b28-d813-4478-a5f5-0ab5c2026e56.png)

Click `launch instance` to create the VM
![image](https://user-images.githubusercontent.com/96930989/222940204-e2f9251b-2f33-4685-8d4e-bf172a1416b0.png)

Download the key pairt and modify other configurations if required
![image](https://user-images.githubusercontent.com/96930989/222948151-35f5c507-61fe-4c73-89c3-be40c2fc3df6.png)

Define the number of instances you want to create and then click launch to create them

![image](https://user-images.githubusercontent.com/96930989/222948299-30569a6b-a73c-4af3-90b4-c30d42ef34b9.png)

![image](https://user-images.githubusercontent.com/96930989/222948360-d537dfd1-083d-48bb-b43d-cd06570ef4dd.png)

### 3. Create billing and free tier usage alerts
![image](https://user-images.githubusercontent.com/96930989/222948445-84403b9a-2099-45f0-bc5e-450fc3843864.png)

Save the settings
![image](https://user-images.githubusercontent.com/96930989/222948576-0c1f3524-3e35-4618-b00c-1ffcbe44d1d7.png)

### 4. Connect the VM

Go back to the VM we created, click connect
![image](https://user-images.githubusercontent.com/96930989/222948651-6393902c-068a-4b9d-bb55-92721b1b0d4b.png)

Username is admin by default
<img width="833" alt="image" src="https://user-images.githubusercontent.com/96930989/222951180-95016539-896f-406b-a3c8-129139a143b9.png">

Using the ssh client to the VM, we will use the key pair downloaded before

