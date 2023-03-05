## Connect AWS to Microsoft defender for cloud

### 1. Create a new [AWS account]((https://www.youtube.com/watch?v=AxsFAuBnQWE))

### 2. [Create VM in AWS](https://www.youtube.com/watch?v=JgfWYcNR6nk)
[AWS Global Accelerator Tool](https://docs.aws.amazon.com/zh_tw/global-accelerator/latest/dg/introduction-speed-comparison-tool.html)

Navigate to EC2
![image](https://user-images.githubusercontent.com/96930989/222940188-39e7d9f4-e5d8-470b-b909-a172e84cb5f1.png)

Create security group if required
![image](https://user-images.githubusercontent.com/96930989/222941703-f503a1c4-8679-423a-aee2-736ee0d344e9.png)

![image](https://user-images.githubusercontent.com/96930989/222941716-15352b28-d813-4478-a5f5-0ab5c2026e56.png)

Click `launch instance` to create the VM
![image](https://user-images.githubusercontent.com/96930989/222940204-e2f9251b-2f33-4685-8d4e-bf172a1416b0.png)

Download the key pair and modify other configurations if required
![image](https://user-images.githubusercontent.com/96930989/222948151-35f5c507-61fe-4c73-89c3-be40c2fc3df6.png)

Define the number of instances you want to create and then click launch to create them

![image](https://user-images.githubusercontent.com/96930989/222948299-30569a6b-a73c-4af3-90b4-c30d42ef34b9.png)

![image](https://user-images.githubusercontent.com/96930989/222948360-d537dfd1-083d-48bb-b43d-cd06570ef4dd.png)

### 3. Create billing and free tier usage alerts
![image](https://user-images.githubusercontent.com/96930989/222948445-84403b9a-2099-45f0-bc5e-450fc3843864.png)

Save the settings
![image](https://user-images.githubusercontent.com/96930989/222948576-0c1f3524-3e35-4618-b00c-1ffcbe44d1d7.png)

### 4. Connect to AWS VM

Go back to the VM we created, click connect
![image](https://user-images.githubusercontent.com/96930989/222948651-6393902c-068a-4b9d-bb55-92721b1b0d4b.png)

Username is admin by default
<img width="833" alt="image" src="https://user-images.githubusercontent.com/96930989/222951180-95016539-896f-406b-a3c8-129139a143b9.png">

Using the ssh client to the VM, we will use the key pair downloaded before

### 5. [Connect AWS to Microsoft defender for cloud](https://www.youtube.com/watch?v=UwYWAClAtgc)

<img width="1157" alt="image" src="https://user-images.githubusercontent.com/96930989/222953576-40f388a9-54e4-498d-b653-08a04e1da7a3.png">

We pick sinle account for example
<img width="890" alt="image" src="https://user-images.githubusercontent.com/96930989/222956985-52c25144-f03e-49ee-9070-432e3237e11e.png">

Select the defender plans you want to enable
<img width="1467" alt="image" src="https://user-images.githubusercontent.com/96930989/222957000-dcac87ab-f95c-487e-aefc-b1d2dd060eb8.png">

Download the template here

<img width="678" alt="image" src="https://user-images.githubusercontent.com/96930989/222957030-78128e50-36cd-42ae-a1e7-53f90dde4ed2.png">

Navigate to AWS, upload the template we got
<img width="1745" alt="image" src="https://user-images.githubusercontent.com/96930989/222957080-6c897f8d-b6ed-45e6-8f74-fb238e5e951e.png">

Specify the name for the stack
<img width="1649" alt="image" src="https://user-images.githubusercontent.com/96930989/222957109-5d4980d9-9f6e-4df3-a040-e9772537661a.png">

Once submit the request, wait until the creation is completed
<img width="1796" alt="image" src="https://user-images.githubusercontent.com/96930989/222957139-20921006-edb5-432b-96d6-ca231f373efb.png">

Once the creation is done, go back to azure portal and go ahead the process
<img width="1826" alt="image" src="https://user-images.githubusercontent.com/96930989/222957256-8b052399-0e5f-47e8-8f76-45b920875551.png">

Once done, you will see the notifications

<img width="540" alt="image" src="https://user-images.githubusercontent.com/96930989/222957269-72c37afb-1f17-40ab-9c13-2b688d7bbb12.png">

The AWS resource will show in defender for cloud

<img width="1815" alt="image" src="https://user-images.githubusercontent.com/96930989/222957336-5dd2fc13-991d-4cab-b91f-3294cb2b89d7.png">
