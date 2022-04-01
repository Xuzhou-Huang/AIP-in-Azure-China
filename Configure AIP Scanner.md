# Configure AIP Scanner Test Environment

### Step 1 - Setup Local AD DS and Create a service account for AIP Scanner

Service account requirements: https://docs.microsoft.com/en-us/azure/information-protection/deploy-aip-scanner-prereqs#service-account-requirements

Grant Log on locally user right assignment by Group Policy:
![image](https://user-images.githubusercontent.com/96280581/161222632-596c3251-0b52-453d-980f-1f705e208102.png)


### Step 2 - Install AAD Connect and Synchronize AIP service accunt to Azure AD


### Step 3 - Install SQL Server - Express

SQL Server requirements: https://docs.microsoft.com/en-us/azure/information-protection/deploy-aip-scanner-prereqs#sql-server-requirements

Download: https://www.microsoft.com/en-us/sql-server/sql-server-downloads

![image](https://user-images.githubusercontent.com/96280581/161218209-05002896-d343-4976-8d73-1cc709bb0c5f.png)

### Step 4 - Install SSMS to manage SQL Server

Download: https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?redirectedfrom=MSDN&view=sql-server-ver15

Connect to SQL Server with SQL Administrator account

![image](https://user-images.githubusercontent.com/96280581/161218548-2d4ab24e-f90c-47f9-bd40-db6fc1e4c70a.png)

Assign **sysadmin** role to AIP service account

Steps referring to https://support.accessdata.com/hc/en-us/articles/203631845-How-to-add-a-service-account-to-Microsoft-SQL-Server

![image](https://user-images.githubusercontent.com/96280581/161219684-f5146ce6-e493-4628-a25a-dbc5069af578.png)

### Step 4 - Install AIP Client

> You must have the AIP unified labeling client installed on your machine before installing the scanner. Do not install the client with just the PowerShell module.

Configure the following registry key to point AIP Client to the correct sovereign cloud for Azure China:

  Registry key  | Type | Name | Value
  ------------- | ------------- | ------------- | -------------
  HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\MSIP |  REG_DWORD | CloudEnvType | 6



Use an account that has local administrator rights and that has permissions to write to the SQL Server master database. An account with Sysadmin role to install the scanner.
