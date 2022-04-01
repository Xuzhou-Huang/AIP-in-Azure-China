# AIP in Azure China

## It's not supported to protect files without AIP Client Installed

  If there's no AIP Client installed, there would be no sensitivity button to apply labels. 

  **word**
  ![image](https://user-images.githubusercontent.com/96280581/160784651-af442433-f97f-4d22-91d8-c95aaf1b0095.png)

  **outlook**
  ![image](https://user-images.githubusercontent.com/96280581/160785060-49e61399-aacf-4186-9a22-f575eac8c786.png)


## Useful AIP Powershell commands

```
PS > Connect-AIPService -EnvironmentName AzureChinaCloud

PS > Get-AipServiceConfiguration | select-Object -ExpandProperty Templates | ft -Autosize

PS > Set-AipServiceTemplateProperty -TemplateID "8dcd174a-be6c-4685-959a-b277079dfed3" -Status Published
```
