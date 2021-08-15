# ARM templates to deploy the spring cloud demo

[Home](../README.MD)

The templates here deploy the following:

* Resource Group
* Key Vault
* Spring Cloud Service
* Spring Cloud Applications
* Azure MySQL


Create a resource group

```powershell
$springResourceGroupOutputs = New-AzDeployment `
-Name (-Join("Deploy-Resource-Group-",(Get-Date).Day,"-",(Get-Date).Month,"-",(Get-Date).Year,"-",(Get-Date).Hour,(Get-Date).Minute))`
-Location "UK South" `
-TemplateFile .\Resource-Group.json -TemplateParameterFile ..\Resource-Group.parameters.json
```

```powershell
$keyVaultOutputs = New-AzResourceGroupDeployment -


```