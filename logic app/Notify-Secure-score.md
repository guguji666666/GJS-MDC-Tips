# Email notification for secure score

### 1. Create a new logic app with consumption plan
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/8a8f05f3-40dd-499d-83a4-9dc91160f8a0)


### 2. Edit in logic app designer, create recurrence so that the logic app runs everyday
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/02ff0264-5425-400b-a7ef-6822cb010e9f)


### 3. Add the HTTP operation > HTTP with Microsoft Entra ID (preauthorized)
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/2ea1bdc1-39b9-4bb5-ad94-255424360d46)


* Method : POST
* Url of the request : `https://management.azure.com/providers/Microsoft.ResourceGraph/resources?api-version=2022-10-01`
* Body of the request : (in json format)
```json
{"query":"resourcecontainers
| where type == \"microsoft.resources/subscriptions\"
| extend subscriptionId = tostring(split(id, \"/\")[2])
| project name, subscriptionId
| join kind=inner (SecurityResources
    | where type == \"microsoft.security/securescores\"    
    | extend current = properties.score.current, max = todouble(properties.score.max)    
    | project subscriptionId, current, max, percentage = ((current / max)*100)    
    | extend subscriptionId = tostring(subscriptionId)
    ) on subscriptionId
| project subscriptionId, name, current, max, percentage"}
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/5bf2c0c3-8c7d-4ba2-8793-8a8a006ebe53)


### 4. Add the action > Parse json
* Content : Body
* Schema : 
```json
{
    "properties": {
        "count": {
            "type": "integer"
        },
        "data": {
            "items": {
                "properties": {
                    "current": {
                        "type": "number"
                    },
                    "max": {
                        "type": "integer"
                    },
                    "name": {
                        "type": "string"
                    },
                    "percentage": {
                        "type": "number"
                    },
                    "subscriptionId": {
                        "type": "string"
                    }
                },
                "required": [
                    "subscriptionId",
                    "name",
                    "current",
                    "max",
                    "percentage"
                ],
                "type": "object"
            },
            "type": "array"
        },
        "totalRecords": {
            "type": "integer"
        }
    },
    "type": "object"
}
```
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/aa713cb9-4286-4010-b563-0a264acda43d)


### 5. Add the action > Create HTML table
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/ee0d3474-1832-44ad-93ca-9aa323fac875)


### 6. Add the action > Send an email, configure body, subject and mailboxes which you want to receive the notification.
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/4af68f8d-b5b1-4380-a10e-864f4ad80a5a)

#### The output in mail looks like
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/73ff01c8-08b1-416c-a993-af253176fe37)

