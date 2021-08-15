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
-TemplateFile .\Resource-Group.json -TemplateParameterFile .\Resource-Group.parameters.json
```

Deploy a Key Vault

```powershell
$keyVaultOutputs = New-AzResourceGroupDeployment `
-Name (-Join("Deploy-Key-Vault-",(Get-Date).Day,"-",(Get-Date).Month,"-",(Get-Date).Year,"-",(Get-Date).Hour,(Get-Date).Minute)) `
-ResourceGroupName $springResourceGroupOutputs.Outputs.resourceGroup_Name.value `
-TemplateFile .\Key-Vault.json `
-TemplateParameterFile .\Key-Vault.parameters.json
```