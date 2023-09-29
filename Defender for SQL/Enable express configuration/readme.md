# Enable defender for SQL express configuration

## Azure CLI

### Get current status
```powershell
az rest --method Get --uri /subscriptions/<subscription id>/resourceGroups/<RG name>/providers/Microsoft.Sql/servers/<SQL server name>/sqlVulnerabilityAssessments/default?api-version=2022-02-01-preview
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/49d71f87-12c1-4b2c-a61a-a772a4b703fa)

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/7d381a7d-ccaf-45db-b21b-4ab6e8b2650d)

### Disable express configuration
```powershell
az rest --method PUT --uri "/subscriptions/<subscription id>/resourceGroups/<RG name>/providers/Microsoft.Sql/servers/<SQL server name>/sqlVulnerabilityAssessments/default?api-version=2022-02-01-preview" --body '{
    "properties": {
      "state": "Disabled"
    }
  }'
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/f8d5afe9-f405-4ecb-804b-486c92e8ec6c)

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/1fba3205-9b60-458c-9f06-265f9ded8e72)

### Enable express configuration
```powershell
az rest --method PUT --uri "/subscriptions/<subscription id>/resourceGroups/<RG name>/providers/Microsoft.Sql/servers/<SQL server name>/sqlVulnerabilityAssessments/default?api-version=2022-02-01-preview" --body '{
    "properties": {
      "state": "Enabled"
    }
  }'
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/653c3337-2bca-4369-9bd6-30559e660dc5)

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/4dea70ee-bc21-4872-806e-c77f5d32b44b)

