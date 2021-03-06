# Configure AIP for Azure China default domain (partner.onmschina.cn)

### Step 1 - Create the *Microsoft Information Protection Sync Service* service principal

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

### Step 2 - Point to Azure China Environment to download label and label policies

AIP apps on Windows need the following registry key to point them to the correct sovereign cloud for Azure China:

  Registry key  | Type | Name | Value
  ------------- | ------------- | ------------- | -------------
  HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\MSIP |  REG_DWORD | CloudEnvType | 6

### Step 3 - Connect to your tenant's RMS Service

**Connection to RMS service would fail with default configuration**

  Logs under _C:\Users\\\<username>\AppData\Local\Microsoft\MSIPC\Logs_ show the following errors:

  ```
  }}{{[325][msipc]:[Info]:[9080]:[2022-03-30 08:11:05.999]: dnslookupclient.cpp:Microsoft::InformationProtection::DnsLookupClient::LookupDiscoveryService:94`

  DNS Lookup failed looking up record for _rmsredir._http._tcp.aadfederation.partner.onmschina.cn with 9003`

  }}{{[326][msipc]:[Info]:[9080]:[2022-03-30 08:11:06.015]: dnslookupclient.cpp:Microsoft::InformationProtection::DnsLookupClient::LookupDiscoveryService:94`

  DNS Lookup failed looking up record for _rmsredir._http._tcp.partner.onmschina.cn with 9003`

  }}{{[327][msipc]:[Info]:[9080]:[2022-03-30 08:11:06.046]: dnslookupclient.cpp:Microsoft::InformationProtection::DnsLookupClient::LookupDiscoveryService:94`

  DNS Lookup failed looking up record for _rmsredir._http._tcp.onmschina.cn with 9003`
  ```


**Manually create registry keys to Connect to 21V RMS service**

  Manually create the following two registry keys, referring to https://docs.microsoft.com/en-us/azure/information-protection/rms-client/client-deployment-notes.

  Registry key  | Type | Name | Value
  ------------- | ------------- | ------------- | -------------
  HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSIPC\ServiceLocation\EnterpriseCertification | REG_SZ | default | https://RMS_Service_ID/_wmcs/Certification
  HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSIPC\ServiceLocation\EnterprisePublishing  | REG_SZ | default | https://RMS_Service_ID/_wmcs/Licensing

  Connect to AIP service via Powershell to check your RMS Service Id

  ```
  PS > Connect-AIPService -EnvironmentName AzureChinaCloud

  A connection to the Azure Information Protection service was opened.

  PS > Get-AipServiceConfiguration

  BPOSId                                    : 97195526-XXXX-XXXX-XXXX-36faa3980a03
  RightsManagementServiceId                 : b5fb3598-XXXX-XXXX-XXXX-b575b91b95a7
  LicensingIntranetDistributionPointUrl     : https://b5fb3598-XXXX-XXXX-XXXX-b575b91b95a7.rms.aadrm.cn/_wmcs/licensing
  LicensingExtranetDistributionPointUrl     : https://b5fb3598-XXXX-XXXX-XXXX-b575b91b95a7.rms.aadrm.cn/_wmcs/licensing
  CertificationIntranetDistributionPointUrl : https://b5fb3598-XXXX-XXXX-XXXX-b575b91b95a7.rms.aadrm.cn/_wmcs/certification
  CertificationExtranetDistributionPointUrl : https://b5fb3598-XXXX-XXXX-XXXX-b575b91b95a7.rms.aadrm.cn/_wmcs/certification
  AdminConnectionUrl                        : https://admin.aadrm.cn/admin/admin.svc/Tenants/b5fb3598-XXXX-XXXX-XXXX-b575b91b95a7
  AdminV2ConnectionUrl                      : https://admin.aadrm.cn/adminV2/admin.svc/Tenants/b5fb3598-XXXX-XXXX-XXXX-b575b91b95a7
  OnPremiseDomainName                       : 
  Keys                                      : {c5c2fa0f-XXXX-XXXX-XXXX-a6a7b38519e7}
  CurrentLicensorCertificateGuid            : c5c2fa0f-XXXX-XXXX-XXXX-a6a7b38519e7
  Templates                                 : {f8c88491-e2fb-4f0f-9d5a-4e652aa673c9, 6b2ca8ca-0165-4dbd-a10c-ec13443e98f2}
  FunctionalState                           : Enabled
  SuperUsersEnabled                         : Disabled
  SuperUsers                                : {}
  AdminRoleMembers                          : {}
  KeyRolloverCount                          : 0
  ProvisioningDate                          : 3/29/2022 5:10:33 AM
  IPCv3ServiceFunctionalState               : Enabled
  DevicePlatformState                       : {Windows -> True, WindowsStore -> True, WindowsPhone -> True, Mac -> True...}
  FciEnabledForConnectorAuthorization       : True
  DocumentTrackingFeatureState              : Enabled
  ```

### Step 4 - Reset AIP Client and it could be recovered to get label and templates

**word**
![image](https://user-images.githubusercontent.com/96280581/160986533-b083784d-ac9a-4899-b6fa-2dd07edb8298.png)

**outlook**
![image](https://user-images.githubusercontent.com/96280581/160989184-c022d202-acd3-417d-8b98-d9c02ccc0196.png)

