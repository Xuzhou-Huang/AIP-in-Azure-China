# Setup AIP Client with 21V default domain

# Protect files without AIP Client Installed

## Protection would fail by default

If there's no AIP Client installed, there would be no sensitivity button to apply labels. 

word:
![image](https://user-images.githubusercontent.com/96280581/160784651-af442433-f97f-4d22-91d8-c95aaf1b0095.png)

outlook:
![image](https://user-images.githubusercontent.com/96280581/160785060-49e61399-aacf-4186-9a22-f575eac8c786.png)

It would also fail to connect with RMS service to apply template **by default**. 

Logs under _C:\Users\\\<username>\AppData\Local\Microsoft\MSIPC\Logs_ show the following errors:

```
}}{{[325][msipc]:[Info]:[9080]:[2022-03-30 08:11:05.999]: dnslookupclient.cpp:Microsoft::InformationProtection::DnsLookupClient::LookupDiscoveryService:94`

DNS Lookup failed looking up record for _rmsredir._http._tcp.aadfederation.partner.onmschina.cn with 9003`

}}{{[326][msipc]:[Info]:[9080]:[2022-03-30 08:11:06.015]: dnslookupclient.cpp:Microsoft::InformationProtection::DnsLookupClient::LookupDiscoveryService:94`

DNS Lookup failed looking up record for _rmsredir._http._tcp.partner.onmschina.cn with 9003`

}}{{[327][msipc]:[Info]:[9080]:[2022-03-30 08:11:06.046]: dnslookupclient.cpp:Microsoft::InformationProtection::DnsLookupClient::LookupDiscoveryService:94`

DNS Lookup failed looking up record for _rmsredir._http._tcp.onmschina.cn with 9003`
```
## Connect to 21V RMS service by manually creating registry keys

Manually create the following two registry keys, referring to https://docs.microsoft.com/en-us/azure/information-protection/rms-client/client-deployment-notes.

Registry key  | Type | Value
------------- | ------------- | -------------
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSIPC\ServiceLocation\EnterpriseCertification | REG_SZ: default | https://RMS_Service_ID/_wmcs/Certification
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSIPC\ServiceLocation\EnterprisePublishing  | REG_SZ: default | https://RMS_Service_ID/_wmcs/Licensing

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

After configuration above, connection with 21V RMS service would be recovered.

