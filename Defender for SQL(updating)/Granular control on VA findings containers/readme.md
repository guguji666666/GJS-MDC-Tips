# Granular control on VA findings containers

As mentioned in the doc [Find and remediate vulnerabilities in your Azure SQL databases](https://learn.microsoft.com/en-us/azure/defender-for-cloud/sql-azure-vulnerability-assessment-find?tabs=classic#find-vulnerabilities-in-your-azure-sql-databases), if you configure the storage account (classic) to save SQL VA findings, then the permissions required for management <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/4483237e-ff55-4d99-8e82-8550db911cd3)


In case that we store cross subscription SQL VA findings in a single storage accounts, we need to restrict the user to have only write permission on the SQL findings from the SQL server they own <br>

First we need to have storage account with `Hierarchical namespace` enabled <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/32731c6e-93d2-4ae3-9c79-05df26db4540)

Then we need to: <br>
* Assign the role `Security Admin` at the subscription level where defender for cloud is enabled
* Assign the role `SQL Security Manager` at the SQL server level
* Assign the role `Storage Blob Data Reader` at the storage account level
* Assign the role `Reader` at the storage account level, or you will meet the error "storage account not found"

Then let's configure the permissions on the containers in storage account. <br>
[Access control lists (ACLs) in Azure Data Lake Storage Gen2](https://learn.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-access-control)

Normally the SQL VA findings are saved under path `vulnerability-assessment / scans / <server name> / <database name>` <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/c56e976e-2271-473a-a4cb-f54bf7a59768)

Then we need to 
1. Assign the `Execute` permission to the user on the container `vulnerability-assessment`

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/b14239ae-58c6-4407-a92d-d2e459da1c72)

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/23a5e2e5-656a-481b-8830-6e4027cd911d)

2. Assign the `Execute` permission to the user on the folder `scans`

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/99653e62-4f29-4c2a-81bc-a15c51c776de)

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/103f79e9-901f-4842-9ff2-2053c0b49fdd)

3. Assign `Read`,`Write` and `Execute` permissions to the user on the folder `<your SQL server name>

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/9e1c9f56-2bff-489e-b364-a0aa63b5b335)

4. Assign `Read`,`Write` and `Execute` permissions to the user on the folder `<your SQL databases>

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/a51ce63e-b340-4c9e-99bc-6427a0c7aa3f)

In this test, i only assign the permissions to user on the database `guguji-sqldb01`, for `master` database i didn't assign any permissions

SQL DB `guguji-sqldb01` <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/60c629b6-2f8a-4246-9e62-426b28ba07d5)

SQL DB `master` <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/6827b7c5-ee32-4003-bacf-0971c69e55fc)

Then we sign in as the test user account and check the permissions <br>

The test user can't create new folder under `scans` <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/88e30b58-0780-4812-80e8-223a1402da69)

The test user can't create new folder under the folder `sql server name` <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/9ea357bb-92cb-44af-970b-7f72c72ddaae)
