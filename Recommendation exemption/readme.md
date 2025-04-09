# Create exemption in recommendation

## 1. Policy-based recommendation

## 2. Non policy-based recommendation
### 1. [List standards Rest API](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/security-standards/list?view=rest-defenderforcloud-2024-08-01&tabs=HTTP)
```
GET https://management.azure.com/{scope}/providers/Microsoft.Security/securityStandards?api-version=2024-08-01
```
### Scope could be:
* Subscription
* Management group
* Security data connector such as AWS environmnet connected to Azure subscription, there should be a resource group created

In API response, search for the `assessmentkey id` and note the `id` it belongs to <br>

For example
```json
        {
            "properties": {
                "displayName": "Custom AWS standard 222",
                "description": "",
                "standardType": "Custom",
                "assessments": [
                    {
                        "assessmentKey": "a022693c-cfae-43a5-86d7-9bec995fdd9c"
                    },
                    {
                        "assessmentKey": "04e4dc63-ee46-405b-a02a-2a8395fe233d"
                    },
                    {
                        "assessmentKey": "9694d4ef-f21a-40b7-b535-618ac5c5d21e"
                    },
                    {
                        "assessmentKey": "036bb56b-c442-4352-bb4c-5bd0353ad314"
                    }
                ],
                "cloudProviders": [
                    "AWS"
                ]
            },
            "id": "/subscriptions/<sub id>/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/securityStandards/3d79fcde-7862-4ef9-a0d6-98c57cb9f879",
            "name": "3d79fcde-7862-4ef9-a0d6-98c57cb9f879",
            "type": "Microsoft.Security/securityStandards"
        }
```
Note the info below:
* id : the standard id
* assessmentkey: we can find it in recommendation > open query, scroll down to the bottom

![image](https://github.com/user-attachments/assets/dbd0a4b9-3a20-46e2-86dc-d9d1c815b038)

![image](https://github.com/user-attachments/assets/42d360c3-b02d-40e9-89a5-25e381e29022)


### 2. [List standard assignment Rest API](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/standard-assignments/list?view=rest-defenderforcloud-2024-08-01&tabs=HTTP)
```
GET https://management.azure.com/{scope}/providers/Microsoft.Security/standardAssignments?api-version=2024-08-01
```
### Scope could be:
* Subscription
* Management group
* Security data connector such as AWS environmnet connected to Azure subscription, there should be a resource group created

Sample response <br>
```json
{
    "value": [
        {
            "properties": {
                "displayName": "AWS Foundational Security Best Practices",
                "assignedStandard": {
                    "id": "/providers/Microsoft.Security/securityStandards/72a4f5fe-db26-4d81-b922-09644839bd4e"
                },
                "effect": "Audit",
                "metadata": {
                    "createdBy": "00000000-0000-0000-0000-000000000000",
                    "createdOn": "2024-09-09T08:36:35.9673563+00:00",
                    "lastUpdatedBy": "00000000-0000-0000-0000-000000000000",
                    "lastUpdatedOn": "2024-09-09T08:36:35.9673575+00:00"
                }
            },
            "id": "/subscriptions/<sub id>/resourcegroups/MDC-AWS/providers/microsoft.security/securityconnectors/GJS-AWS/providers/Microsoft.Security/standardAssignments/f7ca1c20-5427-43a9-b521-e2d5cc5019ad",
            "name": "f7ca1c20-5427-43a9-b521-e2d5cc5019ad",
            "type": "Microsoft.Security/standardAssignments"
        },
        {
            "properties": {
                "displayName": "Microsoft cloud security benchmark",
                "assignedStandard": {
                    "id": "/providers/Microsoft.Security/securityStandards/284baf5b-b707-4fe8-a140-9ec9681362a9"
                },
                "effect": "Audit",
                "metadata": {
                    "createdBy": "00000000-0000-0000-0000-000000000000",
                    "createdOn": "2024-09-09T08:36:35.9980372+00:00",
                    "lastUpdatedBy": "00000000-0000-0000-0000-000000000000",
                    "lastUpdatedOn": "2024-09-09T08:36:35.9980378+00:00"
                }
            },
            "id": "/subscriptions/<sub id>/resourcegroups/MDC-AWS/providers/microsoft.security/securityconnectors/GJS-AWS/providers/Microsoft.Security/standardAssignments/263f586c-9a2f-460a-9a8b-391d2c7b6ee7",
            "name": "263f586c-9a2f-460a-9a8b-391d2c7b6ee7",
            "type": "Microsoft.Security/standardAssignments"
        },
        {
            "properties": {
                "displayName": "AWS CSPM (Preview)",
                "assignedStandard": {
                    "id": "/providers/Microsoft.Security/securityStandards/953b8223-2772-42e5-a1e6-27b43b67dac9"
                },
                "effect": "Audit",
                "metadata": {
                    "createdBy": "00000000-0000-0000-0000-000000000000",
                    "createdOn": "2024-09-09T08:36:36.0096549+00:00",
                    "lastUpdatedBy": "00000000-0000-0000-0000-000000000000",
                    "lastUpdatedOn": "2024-09-09T08:36:36.0096555+00:00"
                }
            },
            "id": "/subscriptions/<sub id>/resourcegroups/MDC-AWS/providers/microsoft.security/securityconnectors/GJS-AWS/providers/Microsoft.Security/standardAssignments/2ff74203-a9e6-4f57-af59-46b6cf700d94",
            "name": "2ff74203-a9e6-4f57-af59-46b6cf700d94",
            "type": "Microsoft.Security/standardAssignments"
        },
        {
            "properties": {
                "displayName": "testfromsecruityadmin",
                "description": "test from secruityadmin",
                "effect": "Exempt",
                "exemptionData": {
                    "assignedAssessment": {
                        "assessmentKey": "3428e584-0fa6-48c0-817e-6d689d7bb879"
                    },
                    "exemptionCategory": "Mitigated"
                },
                "metadata": {
                    "createdBy": "64849255-9746-4dab-8abd-ffae09fd8b06",
                    "createdOn": "2024-10-06T12:44:46.4767143Z",
                    "lastUpdatedBy": "64849255-9746-4dab-8abd-ffae09fd8b06",
                    "lastUpdatedOn": "2024-10-06T12:44:46.4767146Z"
                }
            },
            "id": "/subscriptions/<sub id>/resourcegroups/mdc-aws/providers/microsoft.security/securityconnectors/gjs-aws/securityentitydata/aws-ec2-vpc-vpc-0cd0ab5473a32d91c-sa-east-1/providers/Microsoft.Security/standardAssignments/28947a1d-5d54-4bae-92f5-fb28e63e1a5c",
            "name": "28947a1d-5d54-4bae-92f5-fb28e63e1a5c",
            "type": "Microsoft.Security/standardAssignments"
        },
        {
            "properties": {
                "displayName": "AWS FedRAMP High Baseline Rev5 (Preview)",
                "description": "This object has been generated by Microsoft Defender for Cloud. To make changes, navigate to the security policies management page.",
                "assignedStandard": {
                    "id": "/providers/Microsoft.Security/securityStandards/8e281af3-9a76-4e3d-90b1-8be01f45d147"
                },
                "effect": "Audit",
                "metadata": {
                    "createdBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "createdOn": "2025-01-23T09:01:45.764285Z",
                    "lastUpdatedBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "lastUpdatedOn": "2025-01-23T09:01:45.7642853Z"
                }
            },
            "id": "/subscriptions/<sub id>/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/standardAssignments/45c59f84-01f2-44ef-b0a7-034ab001a6f3",
            "name": "45c59f84-01f2-44ef-b0a7-034ab001a6f3",
            "type": "Microsoft.Security/standardAssignments"
        },
        {
            "properties": {
                "displayName": "AWS Australian Government Information Security Manual 12.2023 (Preview)",
                "description": "This object has been generated by Microsoft Defender for Cloud. To make changes, navigate to the security policies management page.",
                "assignedStandard": {
                    "id": "/providers/Microsoft.Security/securityStandards/8690c26e-3268-484e-80d0-c492ebfa3ec7"
                },
                "effect": "Audit",
                "metadata": {
                    "createdBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "createdOn": "2025-01-23T09:01:46.2020562Z",
                    "lastUpdatedBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "lastUpdatedOn": "2025-01-23T09:01:46.2020566Z"
                }
            },
            "id": "/subscriptions/<sub id>/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/standardAssignments/0d7eff70-6fd1-40e5-a239-248d0382aca1",
            "name": "0d7eff70-6fd1-40e5-a239-248d0382aca1",
            "type": "Microsoft.Security/standardAssignments"
        },
        {
            "properties": {
                "displayName": "Custom standard for AWS master account 20250123",
                "description": "This object has been generated by Microsoft Defender for Cloud. To make changes, navigate to the security policies management page.",
                "assignedStandard": {
                    "id": "/subscriptions/<sub id>/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/securityStandards/68e4d650-6508-42a7-a6d9-82c9c8ee6cfa"
                },
                "effect": "Audit",
                "metadata": {
                    "createdBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "createdOn": "2025-01-23T09:05:05.9447374Z",
                    "lastUpdatedBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "lastUpdatedOn": "2025-01-23T09:05:05.9447377Z"
                }
            },
            "id": "/subscriptions/<sub id>/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/standardAssignments/d0e11b45-9fdb-441b-8ce6-34947a7712e6",
            "name": "d0e11b45-9fdb-441b-8ce6-34947a7712e6",
            "type": "Microsoft.Security/standardAssignments"
        },
        {
            "properties": {
                "displayName": "Custom AWS standard 2",
                "description": "This object has been generated by Microsoft Defender for Cloud. To make changes, navigate to the security policies management page.",
                "assignedStandard": {
                    "id": "/subscriptions/<sub id>/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/securityStandards/13baafe1-6ccd-456d-bc11-9497dd436770"
                },
                "effect": "Audit",
                "metadata": {
                    "createdBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "createdOn": "2025-01-23T10:01:01.5943572Z",
                    "lastUpdatedBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "lastUpdatedOn": "2025-01-23T10:01:01.5943575Z"
                }
            },
            "id": "/subscriptions/<sub id>/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/standardAssignments/70be84e1-6437-477e-ac4e-cc8f3bbfd5c5",
            "name": "70be84e1-6437-477e-ac4e-cc8f3bbfd5c5",
            "type": "Microsoft.Security/standardAssignments"
        },
        {
            "properties": {
                "displayName": "Custom AWS standard 222",
                "description": "This object has been generated by Microsoft Defender for Cloud. To make changes, navigate to the security policies management page.",
                "assignedStandard": {
                    "id": "/subscriptions/<sub id>/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/securityStandards/3d79fcde-7862-4ef9-a0d6-98c57cb9f879"
                },
                "effect": "Audit",
                "metadata": {
                    "createdBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "createdOn": "2025-01-23T10:03:42.8443224Z",
                    "lastUpdatedBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "lastUpdatedOn": "2025-01-23T10:03:42.8443228Z"
                }
            },
            "id": "/subscriptions/<sub id>/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/standardAssignments/829cfc74-8ce1-4296-a174-d0b7b27c2978",
            "name": "829cfc74-8ce1-4296-a174-d0b7b27c2978",
            "type": "Microsoft.Security/standardAssignments"
        },
        {
            "properties": {
                "displayName": "Test creating AWS custom standard using API",
                "description": "This object has been generated by Microsoft Defender for Cloud. To make changes, navigate to the security policies management page.",
                "assignedStandard": {
                    "id": "/subscriptions/<sub id>/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/securityStandards/5af44f43-096a-4ff8-b628-d8575f81515e"
                },
                "effect": "Audit",
                "metadata": {
                    "createdBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "createdOn": "2025-01-23T10:22:17.0387016Z",
                    "lastUpdatedBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "lastUpdatedOn": "2025-01-23T10:22:17.0387022Z"
                }
            },
            "id": "/subscriptions/<sub id>/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/standardAssignments/17cda9e6-03b2-4220-81bf-240ffc6f0478",
            "name": "17cda9e6-03b2-4220-81bf-240ffc6f0478",
            "type": "Microsoft.Security/standardAssignments"
        },
        {
            "properties": {
                "displayName": "Assign custom standard for AWS resources",
                "description": "Assignment of custom standard for AWS resources",
                "assignedStandard": {
                    "id": "/subscriptions/<sub id>/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/securityStandards/93ad54fb-3c5e-4499-811d-17c9d7976058"
                },
                "effect": "Audit",
                "metadata": {
                    "createdBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "createdOn": "2025-01-24T01:56:28.0712489Z",
                    "lastUpdatedBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "lastUpdatedOn": "2025-01-24T01:56:28.0712501Z"
                }
            },
            "id": "/subscriptions/<sub id>/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/standardAssignments/b688c3b6-1cfe-4ece-b549-cedec45f4734",
            "name": "b688c3b6-1cfe-4ece-b549-cedec45f4734",
            "type": "Microsoft.Security/standardAssignments"
        },
        {
            "properties": {
                "displayName": "Assign custom standard for AWS resources",
                "description": "Assignment of custom standard for AWS resources",
                "assignedStandard": {
                    "id": "/subscriptions/<sub id>/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/securityStandards/33e3fa64-3631-4e49-8148-d9c87623f126"
                },
                "effect": "Audit",
                "metadata": {
                    "createdBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "createdOn": "2025-01-24T03:33:33.8516032Z",
                    "lastUpdatedBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "lastUpdatedOn": "2025-01-24T03:33:33.8516041Z"
                }
            },
            "id": "/subscriptions/<sub id>/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/standardAssignments/2df1ddc9-348e-4add-a51b-b462c9bf6d4d",
            "name": "2df1ddc9-348e-4add-a51b-b462c9bf6d4d",
            "type": "Microsoft.Security/standardAssignments"
        },
        {
            "properties": {
                "displayName": "Test exemption SSH port should be closed on EC2 machines",
                "description": "Test SSH port should be closed on EC2 machines\n",
                "effect": "Exempt",
                "exemptionData": {
                    "assignedAssessment": {
                        "assessmentKey": "ad758f18-69df-4844-bd94-0b53ed4eac51"
                    },
                    "exemptionCategory": "Waiver"
                },
                "metadata": {
                    "createdBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "createdOn": "2025-04-09T06:02:03.8713857Z",
                    "lastUpdatedBy": "cbf5d4b7-3a29-4810-9a0a-51a1ea1ebbff",
                    "lastUpdatedOn": "2025-04-09T06:02:03.8713875Z"
                }
            },
            "id": "/subscriptions/<sub id>/resourcegroups/mdc-aws/providers/Microsoft.Security/securityConnectors/GJS-AWS/securityentitydata/aws-ec2-instance-i-0615ff9ecdd96ceb4-ap-southeast-1/providers/Microsoft.Security/standardAssignments/d417556a-fa9a-4e8e-a434-a856163c075c",
            "name": "d417556a-fa9a-4e8e-a434-a856163c075c",
            "type": "Microsoft.Security/standardAssignments"
        }
    ]
}
```
Note the info below:
* the guid behind standardAssignments, this would be `standardAssignmentName` in `id`, we need it later

### 3. [Create exemption in existing standard assignment](https://learn.microsoft.com/en-us/rest/api/defenderforcloud/standard-assignments/create?view=rest-defenderforcloud-2024-08-01&tabs=HTTP#put-exemption-standard-assignment)
Sample <br>
![image](https://github.com/user-attachments/assets/c89ceb2f-3cc3-4e70-9ce9-2d8b44675dd9)

```
PUT https://management.azure.com/{resourceId which you want apply exemption for}/providers/Microsoft.Security/standardAssignments/{standardAssignmentName}?api-version=2024-08-01
```

Request body
```json
{
    "properties": {
        "displayName": "Test exemption from bruno for AWS connector demo",
        "description": "Exemption description 2",
        "assignedStandard": {
            "id": "/subscriptions/24e13d8f-b834-46d1-b5a2-72ad3f10c7a4/resourceGroups/MDC-AWS/providers/Microsoft.Security/securityConnectors/GJS-AWS/providers/Microsoft.Security/securityStandards/33e3fa64-3631-4e49-8148-d9c87623f126"
        },
        "effect": "Exempt",
        "expiresOn": "2025-05-02T19:50:47.083633Z",
        "exemptionData": {
            "exemptionCategory": "waiver",
            "assignedAssessment": {
                "assessmentKey": "ad758f18-69df-4844-bd94-0b53ed4eac51"
            }
        }
    }
}
```
