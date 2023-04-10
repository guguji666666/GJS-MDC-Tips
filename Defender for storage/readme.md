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

We are able to exclude a specific Storage account if the legacy "Per transaction" pricing plan is enabled

We CAN'T exclude a specific Storage account if the legacy "Per-storage-account plan" (new)" pricing plan is enabled

Refer customer to Can I exclude specific storage accounts from protections in per-storage account pricing? 

### Deploy defender for storage plan with per-transaction pricng mode

[Set up per-transaction pricing for a subscription](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-storage-classic-enable#set-up-microsoft-defender-for-storage-classic)

[Exclude an Azure Storage account protection on a subscription with per-transaction pricing](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-storage-classic-enable#exclude-an-azure-storage-account-protection-on-a-subscription-with-per-transaction-pricing)
