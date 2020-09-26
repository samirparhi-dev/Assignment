# Q2 - SCENARIO
_**Macro Life, a healthcare company has recently setup the entire Network and Infrastructure on Azure. 
The infrastructure has different components such as Virtual N/W, Subnets, NIC, IPs, NSG etc.
The IT team currently has developed Power Shell scripts to deploy each component where all the properties of each resource is set using Power Shell commands.
The business has realized that the Power Shell scripts are growing over period of time and difficult to handover when new admin onboard in the IT.
The IT team has now decided to move to ARM based deployment of all resources to Azure.
All the passwords are stored in a Azure Service known as key Vault. The deployments needs to be automated using Azure DevOps using IaC(Infrastructure as Code).**_

###### 1) What are different artifacts you need to create – name of the artifacts and its purpose

  Below are the required things to create an an ARM template .
* Schema: Describe the properties available in the template
* Content version: Versioning  the template
* Parameters:  Passing parameter for this template
* Variable: Use of variable in this Template dynamically
* Output: Return properties after execution of this template
* Function: Re-usability of the expressions
* Resources(Azure resources like N/w , Subnet, Vm, etc.) : Infra object to be created by execution of this template 

###### 2) List the tools you will to create and store the ARM templates.
* VS code – for developing the ARM template  ( With ARM plugin)
* Git: to store the ARM template with versions
* Powershell: to deploy Arm template with (Azure PS module installed in the local M/c)

###### 3) Explain the process and steps to create automated deployment pipeline.
  * step -1: Create a ARM templates
  * step-2: Store it in Git hub
  * step 3: Create Arm Deployment pipeline in Azure DevOps (Please refer to the earlier assignment to see this process). (It should also contain the Resource group creation part)
  * step-4: Trigger the pipeline whenever required to create IaaC

###### 4) Create a sample ARM template you will use to deploy a Windows VM of any size

    Please find ARMdeployment.json file the git hub.
    
    https://github.com/samirparhi-dev/Assignment/blob/master/Scnario-2/ARMdeployment.json
    
###### 5) Explain how will you access the password stored in Key Vault and use it as Admin Password in the VM ARM template.

* Create a key vault first in azure portal using Key vault service.
* Add the secret there by providing the Name and value with Manual Upload option 
* Create Access Policies for this secret  by adding an User , which you want to give access
* Enable Arm Access by going to advanced policies , so that ARM template can access it
* Declare the “adminPassword” parameter as a secure string type in the Arm template. 

Example:

```json
"adminPassword": {
      "type": "securestring",
      "minLength": 12,
      "metadata": {
        "description": "Password for the AdminUser"
      }
    },
```
* Create a parameter file to access the key value:

Example :

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminPassword": {
      "reference": {
        "keyVault": {
          "id": "/subscriptions/<GUID here>/resourceGroups/<group-name here>/providers/Microsoft.KeyVault/vaults/<vaultname Here>"
        },
        "secretName": "AdminPasswordSecret"
      }
    }
```
* Run the below command in the Azure CLI to include parameter template in the Arm template:

```shell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName TestEnv -TemplateFile .\ArmTemplateFile.json -TemplateParameterFile .\ArmParametersFile.json
```


                                                              **End Of Document**
