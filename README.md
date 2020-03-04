# Azure-Sample-ARM-Template-Architecture
Shows how to build a set of linked templates with conditions, loops, array and complex parameters.  The templates have been developed as a set of nested templates that allows for team collaboration on the templates.  You can author and test each linked (nested) template individually and then after successfully testing, incorporate the template into the master template.  This template allows for the parameters files to the updated to add new VMs, subnets and nsgs.


## How to run
* Download azuredeploy.parameters.json
* Make your edits
* Click the Deploy to Azure button
* Click "Edit Parameters" and upload your azuredeploy.parameters.json


## Actions
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fadampaternostro%2FAzure-Sample-ARM-Template-Architecture%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="http://armviz.io/visualizebutton.png"/></a>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fadampaternostro%2FAzure-Sample-ARM-Template-Architecture%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/> 
</a>

## Overview of Templates
The below diagram shows the layout of the templates.  The templates have been nested such that each nested template can be tested independently and then linked in the master template.  The master template will pass the parameters to each linked template and reference an dependencies between the linked templates.

![alt tag](https://raw.githubusercontent.com/adampaternostro/Azure-Sample-ARM-Template-Architecture/master/ARM-Architecture.png)

### Specific template notes
*  The VM template has the most complex set of parameters
   *  The availabilitySet parameter can be set to an empty string will specifics no availibility set for the VM.
   *  The publicIPAddress parameter can be set to true or false which determines if the NIC card is assigned a public IP address.  Conditions are used in the ARM template to achive this.
   *  The VM template uses just a single parameter file which is shared between the NIC, Public IPs and the VMs.  This simplified some testing.
*  The VNET template does a loop to create each subnet.  The subnets cannot be placed in a seperate linked template since when you run the ARM template a second time it will fail.  The VNET will attempt to remove all the subnets when the subnets are in a seperate template.


### Examples
* azuredeploy.availabilityset uses the copy command to create many exact copies
* azuredeploy.vm-nic uses an "if" statement to conditionally create a sub-resource (or null out the JSON)
* azuredeploy.vnet uses a copy command for a sub-resource


## To Run via command line (Linux)
```
# Login
az login

# Select Subscription
az account set -s REPLACE_ME

# Script parameters
resourceGroup="Azure-Sample-ARM-Template-Architecture"
location="eastus"
today=`date +%Y-%m-%d-%H-%M-%S`
deploymentName="MyDeployment-$today"

# Create resource group
az group create \
  --name        $resourceGroup \
  --location    $location

# Deploy the ARM template
az group deployment create \
  --name                 $deploymentName \
  --resource-group       $resourceGroup \
  --template-file        azuredeploy.json \
  --parameters           @azuredeploy.parameters.json \
  --mode                 Incremental

# Clean up resource group
az group delete --name $resourceGroup
```


## To Run via command line (Powersehll)
```
# Login
Connect-AzAccount

# Select Subscription
$subscriptionId="REPLACE_ME"
$context = Get-AzSubscription -SubscriptionId $subscriptionId
Set-AzContext $context

# Script parameters
$resourceGroup="Azure-Sample-ARM-Template-Architecture"
$location="eastus"
$today=(Get-Date).ToString('yyyy-MM-dd-HH-mm-ss')
$deploymentName="MyDeployment-$today"

# Create resource group
New-AzResourceGroup -Name $resourceGroup -Location $location

# Deploy the ARM template
New-AzResourceGroupDeployment -Name $deploymentName -ResourceGroupName $resourceGroup -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json -Mode Incremental

# Clean up resource group
Remove-AzResourceGroup -Name $resourceGroup 
```



## References
* Each Azure Resource Reference: https://docs.microsoft.com/en-us/azure/templates/
* Quickstart templates: https://github.com/Azure/azure-quickstart-templates
