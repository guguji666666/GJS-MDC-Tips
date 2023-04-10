## Defender for storage

### Check the type of defender for storage plan

```kusto
securityresources 
| where type == "microsoft.security/pricings"
| where subscriptionId == "d681dc2c-9d21-4b07-aa03-28016ca5dff4"
| where ['id'] contains "storage"
| project subscriptionId, id, properties.pricingTier, properties.subPlan
```

![image](https://user-images.githubusercontent.com/96930989/230875188-b9d2bea4-fddc-4498-9dc8-d0150194fe2f.png)


### Plan type

