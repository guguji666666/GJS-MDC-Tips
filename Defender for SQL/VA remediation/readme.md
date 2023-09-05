## SQL VA remediation

[SQL VA results reference list](https://learn.microsoft.com/en-us/azure/defender-for-cloud/sql-azure-vulnerability-assessment-rules#data-protection)

[Find and remediate vulnerabilities in your Azure SQL databases](https://learn.microsoft.com/en-us/azure/defender-for-cloud/sql-azure-vulnerability-assessment-find?tabs=express)

[Manage vulnerability findings in your Azure SQL databases](https://learn.microsoft.com/en-us/azure/defender-for-cloud/sql-azure-vulnerability-assessment-manage?source=recommendations&tabs=express#faq)

[VA1143 - 'dbo' user should not be used for normal service operation](https://learn.microsoft.com/en-us/answers/questions/1187749/va1143-dbo-user-should-not-be-used-for-normal-serv) <br>
We should "Create users with low privileges to access the DB and any data stored in it with the appropriate set of permissions." Make sure you use the "least privilege principal" approach. Give users permissions that are absolutely necessary. Make sure they do not have ALTER database permissions. Make use of Database Roles and assigned users to them. Make sure "dbo" is restricted to administrators only.

VA1219 - Transparent data encryption should be enabled <br>
[Transparent data encryption (TDE) on SQL server](https://learn.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-ver16#enable-tde) <br>
[Transparent data encryption for SQL Database, SQL Managed Instance, and Azure Synapse Analytics](https://learn.microsoft.com/en-us/azure/azure-sql/database/transparent-data-encryption-tde-overview?view=azuresql&tabs=azure-portal)
