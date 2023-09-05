# Granular control on blobs under VA findings container

As mentioned in the doc [Find and remediate vulnerabilities in your Azure SQL databases](https://learn.microsoft.com/en-us/azure/defender-for-cloud/sql-azure-vulnerability-assessment-find?tabs=classic#find-vulnerabilities-in-your-azure-sql-databases), if you configure the storage account (classic) to save SQL VA findings, then the permissions required for management <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/4483237e-ff55-4d99-8e82-8550db911cd3)

In case that we store cross subscription SQL VA findings in a single storage accounts, we need to restrict the user to have only write permission on the SQL findings from the SQL server they own <br>

## 1. Requirement of storage account
First we need to have storage account with `Hierarchical namespace` enabled <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/32731c6e-93d2-4ae3-9c79-05df26db4540)


## 2. Azure RBAC role
Then we need to:
* Assign the role `Security Admin` at the subscription level where defender for cloud is enabled
* Assign the role `SQL Security Manager` at the `SQL server level`
* Assign the role `Storage Blob Data Reader` at the storage account level
* Assign the role `Reader` at the storage account level, or you will meet the error "storage account not found"

If you assign the `Storage Blob Data Reader` only at the blob level as mentioned in [Assign an Azure role for access to blob data](https://github.com/MicrosoftDocs/azure-docs/blob/main/articles/storage/blobs/assign-azure-role-data-access.md), you will get this `error` when check SQL VA findings <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/c4bc3f26-3766-4f91-ac9e-d027599c6eb2)
```
{
    "error": {
        "code": "AuthorizationFailed",
        "message": "The client 'test-sqlva-01@tigaultraman.com' with object id 'xxxxxxxxd4-bcd4-4aec-86a9-f6b71e2b9f2c' does not have authorization to perform action 'Microsoft.Storage/storageAccounts/listKeys/action' over scope '/subscriptions/74a72629-ac6d-44db-a66a-abc69f3bfb7e/resourceGroups/GJS-MS150-MDFC1/providers/Microsoft.Storage/storageAccounts/sqlva01' or the scope is invalid. If access was recently granted, please refresh your credentials."
    }
}
```

In my lab, i have two SQL servers:
* guguji--sql01
* guguji--sql02

I assign the role `SQL Security Manager` to test account on server `guguji--sql01`, for server `guguji--sql02` i didn't assign this role <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/a12d7c39-5ce5-403d-abbc-8766b10e789b) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/9f813fa3-7564-4336-b864-44784a829edb)


## 3. Azure storage ACL
Then let's configure the permissions on the containers in storage account. <br>
[Access control lists (ACLs) in Azure Data Lake Storage Gen2](https://learn.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-access-control)

Normally the SQL VA findings are saved under path `vulnerability-assessment / scans / <server name> / <database name>` <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/c56e976e-2271-473a-a4cb-f54bf7a59768)

Then we need to 
1. Assign the `Execute` permission to the user on the container `vulnerability-assessment` 
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/b14239ae-58c6-4407-a92d-d2e459da1c72) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/23a5e2e5-656a-481b-8830-6e4027cd911d)

2. Assign the `Execute` permission to the user on the folder `scans`
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/99653e62-4f29-4c2a-81bc-a15c51c776de) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/103f79e9-901f-4842-9ff2-2053c0b49fdd)

3. Assign `Read`,`Write` and `Execute` permissions to the user on the folder `<your SQL server name>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/9e1c9f56-2bff-489e-b364-a0aa63b5b335)

4. Assign `Read`,`Write` and `Execute` permissions to the user on the folder `<your SQL databases>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/a729cdaa-e288-4db1-ab3b-ae68be67ed6f) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/3ffdbba2-dcf9-4c26-9a2b-7f0799ed84a3)

Then we sign in as the test user account and check the permissions <br>
The test user can't create new folder under `scans` since the account doesn't have write permission <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/88e30b58-0780-4812-80e8-223a1402da69) <br>

The test account can modify SQL ATP baseline on database `guguji--sql01/guguji-sqldb01` and `master` <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/2cdac16f-8825-4ab1-a6b4-774125046c68) <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/5ba768f2-49df-4753-9f8f-397847b2540c)

### Failure

The test account can't remediate the VA findings on SQL server `guguji-sql02` since it doesn't have the role `SQL Security Manager` assigned <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/9452a594-b5f9-419e-8ccf-358c56206a8e) <br>

In the blob `guguji--sql02`, we didn't assign the `write` permission to the account <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/7651fca5-80d3-4a43-aeef-915cb7565872)

The test account can't create directory under the blob `guguji--sql02` <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/2abbaaff-fef3-450a-95c4-f701dbb19a61)



