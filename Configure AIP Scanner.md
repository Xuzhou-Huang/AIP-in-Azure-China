# Configure AIP Scanner Test Environment

### Step 1 - Setup Local AD DS and Create a service account for AIP Scanner

Service account requirements: https://docs.microsoft.com/en-us/azure/information-protection/deploy-aip-scanner-prereqs#service-account-requirements

Grant Log on locally user right assignment by Group Policy:
![image](https://user-images.githubusercontent.com/96280581/161222632-596c3251-0b52-453d-980f-1f705e208102.png)


### Step 2 - Install AAD Connect and Synchronize AIP Service Account to Azure AD

This synchronized account can be used for delegated access, referring to https://docs.microsoft.com/en-us/azure/information-protection/rms-client/clientv2-admin-guide-powershell#prerequisites-for-running-aip-labeling-cmdlets-unattended.

### Step 3 - Install SQL Server - Express

SQL Server requirements: https://docs.microsoft.com/en-us/azure/information-protection/deploy-aip-scanner-prereqs#sql-server-requirements

Download: https://www.microsoft.com/en-us/sql-server/sql-server-downloads

![image](https://user-images.githubusercontent.com/96280581/161218209-05002896-d343-4976-8d73-1cc709bb0c5f.png)

### Step 4 - Install SSMS and grant sysadmin role to Scanner Service Account

Connect to SQL Server with SQL Administrator and grant **sysadmin** role.

![image](https://user-images.githubusercontent.com/96280581/161259973-850331bb-bac8-429b-846f-49c6531478cb.png)


### Step 4 - Install AIP Client

> You must have the AIP unified labeling client installed on your machine before installing the scanner. Do not install the client with just the PowerShell module.

Configure the following registry key to point AIP Client to the correct sovereign cloud for Azure China:

  Registry key  | Type | Name | Value
  ------------- | ------------- | ------------- | -------------
  HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\MSIP |  REG_DWORD | CloudEnvType | 6

### Step 5 - Install AIP Scanner with Local Administrator

Use an account that has local administrator rights and that has SQL Server master database Sysadmin role to install the scanner.

```
PS > Install-AIPScanner -SqlServerInstance localhost\SQLEXPRESS -Cluster xuhuanAIP
```

When you are prompted, provide the Active Directory credentials for the scanner service account. For example: aip.nowbillions.com\svc_scanner

### Step 6 - Enable AIP scanner to Run Non-interactively

Follow steps listed [here](https://docs.microsoft.com/en-us/azure/information-protection/rms-client/clientv2-admin-guide-powershell#how-to-label-files-non-interactively-for-azure-information-protection) to configure AIP Scanner authentication.

Please be noted that when granting permissions to the AAD Application, you can find the following services under "APIs my organization uses".

Application Name | App Id
------------- | ------------- | 
Microsoft Rights Management Services | 00000012-0000-0000-c000-000000000000
Microsoft Information Protection Sync Service | 870c4f2e-85b6-4d43-bdda-6ed9a579b725


![image](https://user-images.githubusercontent.com/96280581/161228575-a5616b5c-eb05-413e-b311-287bee3966de.png)

![image](https://user-images.githubusercontent.com/96280581/161229349-dbe2c9f4-43f7-439d-8b0f-a1847270722d.png)

If the service account needs to decrypt content, for example, to reprotect files and inspect files that others have protected, make it a super user as follows:

```
PS > Connect-AIPService -EnvironmentName AzureChinaCloud
A connection to the Azure Information Protection service was opened.

PS > Enable-AipServiceSuperUserFeature
The super user feature is enabled for the Azure Information Protection service.

PS > Add-AipServiceSuperUser -EmailAddress "svc_scanner@aip.nowbillions.com" 
svc_scanner@aip.nowbillions.com was added to the list of super users for the Azure Information Protection service.

```

### Step 7 - Manage content scan jobs

Download sample files (with sensitive info) to be scanned from https://github.com/InfoProtectionTeam/Files/blob/master/Scripts/docs.zip

Powershell cmdlets for scanner configuration for your references:

```
Set-AIPScannerConfiguration -OnlineConfiguration Off

Add-AIPScannerRepository -Path 'c:\repoToScan'

start-aipscan

Get-aipscannerstatus
```

Check scanner reports under **%localappdata%\Microsoft\MSIP\Scanner\Reports**

![image](https://user-images.githubusercontent.com/96280581/161262126-855ca795-d496-4c73-a7ea-0248e357a6e2.png)


