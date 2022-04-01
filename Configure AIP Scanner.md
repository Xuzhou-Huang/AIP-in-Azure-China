# Configure AIP Scanner Test Environment

### Step 1 - Setup Local AD DS and Create a service account for AIP Scanner

![image](https://user-images.githubusercontent.com/96280581/161219964-381712f7-dc70-45fd-b834-3df0b5b28c45.png)

### Step 2 - Install AAD Connect and Synchronize AIP service accunt to Azure AD

### Step 3 - Install SQL Server - Express

Download: https://www.microsoft.com/en-us/sql-server/sql-server-downloads

![image](https://user-images.githubusercontent.com/96280581/161218209-05002896-d343-4976-8d73-1cc709bb0c5f.png)

### Step 4 - Install SSMS to manage SQL Server

Download: https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?redirectedfrom=MSDN&view=sql-server-ver15

Connect to SQL Server with SQL Administrator account

![image](https://user-images.githubusercontent.com/96280581/161218548-2d4ab24e-f90c-47f9-bd40-db6fc1e4c70a.png)

Assign **sysadmin** role to AIP service account

Steps referring to https://support.accessdata.com/hc/en-us/articles/203631845-How-to-add-a-service-account-to-Microsoft-SQL-Server

![image](https://user-images.githubusercontent.com/96280581/161219684-f5146ce6-e493-4628-a25a-dbc5069af578.png)
