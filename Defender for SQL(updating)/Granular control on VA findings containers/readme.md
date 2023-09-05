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

