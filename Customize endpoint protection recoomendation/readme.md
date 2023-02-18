## Customize Endpoint Protection Recommendation in Microsoft Defender for Cloud
##### Microsoft Defender for Cloud covers a variety of Antimalware vendors today. You can find the list of supported versions in [Supported endpoint protection solutions](https://learn.microsoft.com/en-us/azure/defender-for-cloud/supported-machines-endpoint-solutions-clouds-servers?tabs=features-windows#supported-endpoint-protection-solutions)
![image](https://user-images.githubusercontent.com/96930989/219843241-37347dd4-510c-4dae-a531-4da58dc993a0.png)

However, in a scenario where you’re using an antimalware protection that is not in the Microsoft Defender for Cloud supported list but you still want to have visibility over the status of this antimalware using Defender for Cloud dashboard,Microsoft Defender for Cloud covers a variety of Antimalware vendors today. You can find the list of supported versions in this article. However, in a scenario where you’re using an antimalware protection that is not in the Microsoft Defender for Cloud supported list but you still want to have visibility over the status of this antimalware using Defender for Cloud dashboard,.
The the steps mentioned below will be a workaround.

### Steps for deployment https://techcommunity.microsoft.com/t5/microsoft-defender-for-cloud/customizing-endpoint-protection-recommendation-in-microsoft/ba-p/1733217
#### 1. Deploy guest configuration extension and enable managed identity on the VM
To audit settings inside a machine, the Guest Configuration extension and a managed identity is required to audit Azure virtual machines. 

To deploy the extension at scale, make sure to choose appropriate scope and assign the policy initiative `Deploy prerequisites to enable Guest Configuration policies on virtual machines` under Azure Policies as shown in the image below.
![image](https://user-images.githubusercontent.com/96930989/219843355-37f9d15a-0f1b-48ef-93ab-9b3f96c7e4e5.png)

Check what's inside the policies
![image](https://user-images.githubusercontent.com/96930989/219843366-5ee14294-b364-48c6-b5a2-ab7c2c67b449.png)

Check the scope during assignment
![image](https://user-images.githubusercontent.com/96930989/219843387-793aaa1d-ff67-4ab2-95c7-384febacec93.png)

Deploy `remediation task` so that the guest configuration extension could be deployed on `existing VMs` as well (not only new VMs created in the future)
![image](https://user-images.githubusercontent.com/96930989/219843400-99ceb4aa-2252-451c-b6e1-b0a7d1d4a7b0.png)

