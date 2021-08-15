# ARM templates to deploy the spring cloud demo

[Home](../README.md)

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

Create secrets for MySQL

```powershell
. .\functions.ps1

addSecretToKeyVault -kvName $keyVaultOutputs.Outputs.keyVault_Name.value -secretName "MySQLAdministrator-Username" -secretType "Username" -secretValue "dbAdmin"
addSecretToKeyVault -kvName $keyVaultOutputs.Outputs.keyVault_Name.value -secretName "MySQLAdministrator-Password" -secretType "Password"

# Retrieve the values from the key vault or add these the parameter file
Write-Output ('Add this: {0} as the Key Vault Id' -f $keyVaultOUtputs.Outputs.keyVault_Id.value)
Write-Output ('Add this: builtInAdmin-Username as the adminUsername secret name')
Write-Output ('Add this: builtInAdmin-Password as the adminPassword secret name')

$adminUsername = Get-AzKeyVaultSecret `
-VaultName $keyVaultOutputs.Outputs.keyVault_Name.value `
-Name "MySQLAdministrator-Username" `
-AsPlainText

$adminPassword = Get-AzKeyVaultSecret `
-VaultName $keyVaultOutputs.Outputs.keyVault_Name.value `
-Name "MySQLAdministrator-Password"
```

and add the GitHub Personal Access Token to Key Vault

```powershell


```