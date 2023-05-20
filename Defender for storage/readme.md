## Defender for storage

### Check the type of defender for storage plan

```kusto
securityresources 
| where type == "microsoft.security/pricings"
| where subscriptionId == "d681dc2c-9d21-4b07-aa03-28016ca5dff4"
| where ['id'] contains "storage"
| project subscriptionId, id, properties.pricingTier, properties.subPlan
```

![image](https://user-images.githubusercontent.com/96930989/230877193-f43bda35-a282-48f0-b3c2-df8543d15a04.png)

![image](https://user-images.githubusercontent.com/96930989/230875611-5a3abf0b-6c47-480d-bfe5-20fe76bf1dc6.png)


### Deploy defender for storage plan with per-transaction pricng mode

We are able to exclude a specific Storage account if the legacy `"Per transaction"` pricing plan is enabled

We CAN'T exclude a specific Storage account if the legacy `"Per-storage-account plan"` (new)" pricing plan is enabled

Refer customer to Can I exclude specific storage accounts from protections in per-storage account pricing? 

[Set up per-transaction pricing for a subscription](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-storage-classic-enable#set-up-microsoft-defender-for-storage-classic)

[Exclude an Azure Storage account protection on a subscription with per-transaction pricing](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-storage-classic-enable#exclude-an-azure-storage-account-protection-on-a-subscription-with-per-transaction-pricing)

### Test mailware scanning

Legacy per-storage mode <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/a5307e41-bf0c-4f53-a4ba-397286e94f75)

Enable per-account mode and turn on on-upload malware scanning
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/5fd85903-bf35-4cc4-ad9e-36a8d3c1dc8a) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/1019cebf-8898-4e94-a140-3ba6cb8db630) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/70fa06de-3e81-4d76-b3ce-d3931a9ff179)


[Create a container](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal#create-a-container) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/42577706-895a-41ca-b0ce-588091444f3b) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/e643f48d-57d8-44d3-a7a3-0a8e8efe799e) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/fe02425f-6160-4db6-9b4c-4bd2657c4305)

[Upload a block blob](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal#upload-a-block-blob) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/5a213855-e25c-41ca-a1b9-474da3514d4d) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/36a2ff9c-b595-4353-adfb-bcf8e401c3c1)

[Testing Malware Scanning](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-storage-test#testing-malware-scanning) <br>
Click the file uploaded, two new tags are found: <br>
`Malware Scanning scan result` and `Malware Scanning scan time` <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/7bc49277-197e-4c7f-8c38-2cedc430a6d9) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/6faf9d4f-07da-4f62-9635-10275abd07c8)



