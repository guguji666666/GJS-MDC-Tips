## Use API to manage Defender for cloud alert suppression rules (subscription level)

### 1. [List existing alert suppression rules](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/alerts-suppression-rules/list?tabs=HTTP)

Select the subscription where the suppression rules locate
![image](https://user-images.githubusercontent.com/96930989/222016636-21277c88-064c-447e-a68f-314e6c7308c0.png)

The output will show the existing suppression rules (we need to copy the body as the sample)
![image](https://user-images.githubusercontent.com/96930989/222016750-5f2ecfd4-87b4-4efc-9f8f-4363c9b8bb82.png)


### 2. [Get details of specified alert suppression rule](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/alerts-suppression-rules/get?tabs=HTTP)

### 3. [Create new alert suppression rule or update existing alert suppression rules](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/alerts-suppression-rules/update?tabs=HTTP)
![image](https://user-images.githubusercontent.com/96930989/221884648-760f1b45-daae-471f-b634-cd0a4f3d6adb.png)

### 4. Define the name of the suppression rule and the subscription where the new rule locates
![image](https://user-images.githubusercontent.com/96930989/225260988-fe555960-44df-47f0-94eb-752a626fdd9b.png)

The body from my sample (we used the part we copied before):
```json
{
    "properties": {
        "alertType": "SIMULATED_VM.Windows_PetyaRansomware",
        "lastModifiedUTC": "2023-02-28T09:00:13.1151574Z",
        "expirationDateUTC": "2024-04-24T05:45:59Z",
        "state": "Enabled",
        "reason": "SpecificEntityFalsePositive",
        "comment": "False Positive",
        "suppressionAlertsScope": {
          "allOf": [
            {
              "contains": "testhostname",
              "field": "entities.host.hostname"
       }
      ]
    }
  }
}
```

### 5. Once we get the response 200 we can check the new suppression rule in the portal
![image](https://user-images.githubusercontent.com/96930989/225261583-d1de9df6-dd6e-4f0a-8b9e-64adb9340cce.png)

![image](https://user-images.githubusercontent.com/96930989/225261605-35c9d09a-44a6-440e-8565-49e0bfda1dc7.png)
