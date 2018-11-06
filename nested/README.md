## This is for running a nested template locally
You need to sign in once and set your subscription once.

## Login, set subscription and resource parameters (only need to do once per session)
```
az login
az account set -s REPLACE_ME
resourceGroup="Azure-Sample-ARM-Template-Architecture"
location="eastus"
az group create --name $resourceGroup --location $location
```

## Test each nested template seperately
You can run each of the below blocks from your local development machine
```
today=`date +%Y-%m-%d-%H-%M-%S`
deploymentName="MyDeployment-$today"
az group deployment create \
  --name                 $deploymentName \
  --resource-group       $resourceGroup \
  --template-file        azuredeploy.nsg.json \
  --parameters           @local.parameters.nsg.json

today=`date +%Y-%m-%d-%H-%M-%S`
deploymentName="MyDeployment-$today"
az group deployment create \
  --name                 $deploymentName \
  --resource-group       $resourceGroup \
  --template-file        azuredeploy.vnet.json \
  --parameters           @local.parameters.vnet.json

today=`date +%Y-%m-%d-%H-%M-%S`
deploymentName="MyDeployment-$today"
az group deployment create \
  --name                 $deploymentName \
  --resource-group       $resourceGroup \
  --template-file        azuredeploy.subnet.json \
  --parameters           @local.parameters.subnet.json    

today=`date +%Y-%m-%d-%H-%M-%S`
deploymentName="MyDeployment-$today"
az group deployment create \
  --name                 $deploymentName \
  --resource-group       $resourceGroup \
  --template-file        azuredeploy.storage.json \
  --parameters           @local.parameters.storage.json   

today=`date +%Y-%m-%d-%H-%M-%S`
deploymentName="MyDeployment-$today"
az group deployment create \
  --name                 $deploymentName \
  --resource-group       $resourceGroup \
  --template-file        azuredeploy.cosmosdb.json \
  --parameters           @local.parameters.cosmosdb.json   

today=`date +%Y-%m-%d-%H-%M-%S`
deploymentName="MyDeployment-$today"
az group deployment create \
  --name                 $deploymentName \
  --resource-group       $resourceGroup \
  --template-file        azuredeploy.streamanalytics.json \
  --parameters           @local.parameters.streamanalytics.json   

today=`date +%Y-%m-%d-%H-%M-%S`
deploymentName="MyDeployment-$today"
az group deployment create \
  --name                 $deploymentName \
  --resource-group       $resourceGroup \
  --template-file        azuredeploy.postgresql.json \
  --parameters           @local.parameters.postgresql.json   

today=`date +%Y-%m-%d-%H-%M-%S`
deploymentName="MyDeployment-$today"
az group deployment create \
  --name                 $deploymentName \
  --resource-group       $resourceGroup \
  --template-file        azuredeploy.iothub.json \
  --parameters           @local.parameters.iothub.json   

today=`date +%Y-%m-%d-%H-%M-%S`
deploymentName="MyDeployment-$today"
az group deployment create \
  --name                 $deploymentName \
  --resource-group       $resourceGroup \
  --template-file        azuredeploy.availabilityset.json \
  --parameters           @local.parameters.availabilityset.json   
 

############################################################
# This is the same parameter file over and over for the VMs
# You will see the same three parameters passed even though they may not be used.  It was to keep things simple.
############################################################

today=`date +%Y-%m-%d-%H-%M-%S`
deploymentName="MyDeployment-$today"
az group deployment create \
  --name                 $deploymentName \
  --resource-group       $resourceGroup \
  --template-file        azuredeploy.vm-public-ip.json \
  --parameters           @local.parameters.vm.json   

today=`date +%Y-%m-%d-%H-%M-%S`
deploymentName="MyDeployment-$today"
az group deployment create \
  --name                 $deploymentName \
  --resource-group       $resourceGroup \
  --template-file        azuredeploy.vm-nic.json \
  --parameters           @local.parameters.vm.json  

today=`date +%Y-%m-%d-%H-%M-%S`
deploymentName="MyDeployment-$today"
az group deployment create \
  --name                 $deploymentName \
  --resource-group       $resourceGroup \
  --template-file        azuredeploy.vm.json \
  --parameters           @local.parameters.vm.json  

```