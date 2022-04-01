# AIP in Azure China

## It's not supported to protect files without AIP Client Installed

  If there's no AIP Client installed, there would be no sensitivity button to apply labels. 

  **word**
  ![image](https://user-images.githubusercontent.com/96280581/160784651-af442433-f97f-4d22-91d8-c95aaf1b0095.png)

  **outlook**
  ![image](https://user-images.githubusercontent.com/96280581/160785060-49e61399-aacf-4186-9a22-f575eac8c786.png)


## AIP Client setup

To use AIP in Azure China, you need pay special attention to the differences with Global Azure referring to https://docs.microsoft.com/en-us/microsoft-365/admin/services-in-china/parity-between-azure-information-protection?view=o365-21vianet#configure-dns-encryption---windows.

You may struggle with setup lab environment for AIP China, hope the notes below could help you on this:

- [Configure AIP Client for Azure China default domain](https://github.com/Xuzhou-Huang/AIP-in-Azure-China/blob/main/Configure%20AIP%20for%20Azure%20China%20default%20domain.md)
- [Configure AIP Client for Custom Domain](https://github.com/Xuzhou-Huang/AIP-in-Azure-China/blob/main/Configure%20AIP%20for%20Custom%20Domain.md)
- [Lab Guidance for AIP Scanner](https://github.com/Xuzhou-Huang/AIP-in-Azure-China/blob/main/Configure%20AIP%20Scanner.md)

## Useful AIP Powershell cmdlet

```
PS > Connect-AIPService -EnvironmentName AzureChinaCloud

PS > Get-AipServiceConfiguration | select-Object -ExpandProperty Templates | ft -Autosize

PS > Set-AipServiceTemplateProperty -TemplateID "8dcd174a-xxxx-xxxx-xxxx-b277079dfed3" -Status Published
```
