# Configure AIP for custom domain

### Step 1 - Verify Custom Domain on Azure Portal

Go to https://portal.azure.cn verify your custom domain by adding DNS record.

### Step 2 - Complete Setup in M365 for Exchange and Exchange Online Protection

Go to https://portal.partner.microsoftonline.cn/ and follow the steps to corresponding DNS records.

![image](https://user-images.githubusercontent.com/96280581/161053807-6133e2b3-824e-4c17-a43f-888707aa03ed.png)

The following DNS records are required.

![image](https://user-images.githubusercontent.com/96280581/161068334-487ec52b-e89d-4b65-8a7c-3294d22399d9.png)

### Step 3 - Add DNS record for RMS discovery

Add the following SRV records, referring to [doc](https://docs.microsoft.com/en-us/microsoft-365/admin/services-in-china/parity-between-azure-information-protection?view=o365-21vianet#configure-dns-encryption---windows).

![image](https://user-images.githubusercontent.com/96280581/164355612-16dad28a-73dc-4101-9de6-c19cad881fba.png)

This DNS record is used during RMS discovery as follows:

```
  
  }}{{[325][msipc]:[Info]:[8724]:[2022-04-01 01:31:14.982]: dnslookupclient.cpp:Microsoft::InformationProtection::DnsLookupClient::LookupDiscoveryService:103

DNS Lookup succeeded looking up record for _rmsredir._http._tcp.aip.nowbillions.com

}}{{[328][msipc]:[Info]:[8724]:[2022-04-01 01:31:14.982]: servicelocation.cpp:CServiceLocation::GetAllUrls:429

+++++++++++ Service discovery using ++++++++
       Token type: OAUTH2
       Intranet Url: https://b5fb3598-318a-4d18-846b-b575b91b95a7.rms.aadrm.cn/_wmcs/servicediscovery
       Extranet Url: NULL
       
 ```

### Step 4 - Create the *Microsoft Information Protection Sync Service* service principal

  The Microsoft Information Protection Sync Service service principal is not available in Azure China tenants by default, and is required for Azure Information Protection.

  The following command only need to be run **for once**. If your tenant already have this service principal, please skip this step.

  ```
  PS > Connect-azaccount -environmentname azurechinacloud

  Account                                   SubscriptionName             TenantId                             Environment    
  -------                                   ----------------             --------                             -----------    
  xuhuan@xxx.partner.onmschina.cn AVD with AAD Dual Federation 97195526-XXXX-XXXX-XXXX-36faa3980a03 AzureChinaCloud

  PS > New-AzADServicePrincipal -ApplicationId 870c4f2e-85b6-4d43-bdda-6ed9a579b725

  DisplayName                                   Id                                   AppId                               
  -----------                                   --                                   -----                               
  Microsoft Information Protection Sync Service 7c479261-XXXX-XXXX-XXXX-dd119d4634af 870c4f2e-85b6-4d43-bdda-6ed9a579b725
  ```

### Step 5 - Point to Azure China Environment to download label and label policies

AIP apps on Windows need the following registry key to point them to the correct sovereign cloud for Azure China:

  Registry key  | Type | Name | Value
  ------------- | ------------- | ------------- | -------------
  HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\MSIP |  REG_DWORD | CloudEnvType | 6


### Step 6 - Logon to AIP Client and verify if labels and templates can be obtained

 ![image](https://user-images.githubusercontent.com/96280581/161203950-d215717a-63c1-406c-b48f-897517131f42.png)

