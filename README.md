# Azure Spring Cloud Workshop
[Microsoft Learn](https://docs.microsoft.com/en-us/learn/modules/azure-spring-cloud-workshop/?WT.mc_id=cloudskillschallenge_8351edfe-a67a-46d4-81cd-6439844b72ac)

[GitHub Indepth](https://github.com/microsoft/azure-spring-cloud-training)

Deploy the infrastructure

[ARM](/ARM/readme.md)

Deploy the Java applications

[Prereqs](https://github.com/microsoft/azure-spring-cloud-training/tree/master/00-setup-your-environment)

```powershell
# Requires the prerequisites defined [here]() have been installed.
# Todo-Service
cd todo-service

./mvnw clean package -DskipTests

# The application output is an array, to get the actual values you must convert
# the json document to a string.
$Apps = $springOutputs.Outputs.springCloudApp_Name.value.ToString() | ConvertFrom-Json
$appToDeploy = $apps | Out-GridView -Title "Select the App" -PassThru

az spring-cloud app deploy --name $appToDeploy `
    --service $springOutputs.Outputs.springCloudService_Name.value `
    --resource-group $springResourceGroupOutputs.Outputs.resourceGroup_Name.value `
    --jar-path target/msdn-training-service-1.0.jar

# Todo-Gateway

cd ..\todo-gateway

./mvnw clean package -DskipTests

az spring-cloud app deploy --name $springOutputs.Outputs.springCloudAppGateway_Name.value `
    --service $springOutputs.Outputs.springCloudService_Name.value `
    --resource-group $springResourceGroupOutputs.Outputs.resourceGroup_Name.value `
    --jar-path target/msdn-training-gateway-1.0.jar
```