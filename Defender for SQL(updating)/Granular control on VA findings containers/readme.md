# Granular control on VA findings containers

As mentioned in the doc [Find and remediate vulnerabilities in your Azure SQL databases](https://learn.microsoft.com/en-us/azure/defender-for-cloud/sql-azure-vulnerability-assessment-find?tabs=classic#find-vulnerabilities-in-your-azure-sql-databases), if you configure the storage account (classic) to save SQL VA findings, then the permissions required for management <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/4483237e-ff55-4d99-8e82-8550db911cd3)

In case that we store cross subscription SQL VA findings in a single storage accounts, we need to restrict the user to have only write permission on the SQL findings from the SQL server they own <br>

Then we need to:
* Assign the role `Security Admin` at the subscription level where defender for cloud is enabled
* Assign the role `SQL Security Manager` at the SQL server level
* Assign the role `Storage Blob Data Reader` at the storage account level
* Assign the role `Reader` at the storage account level, or you will meet the error "storage account not found"
