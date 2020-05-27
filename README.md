# azurerm-getrandom
Creation of Random seed using osDisk names when randomly generated from Image.
Output of this template will then be used as a seed by guid ARM function to generate Managed Identity Roles.

## UPDATE!!! May, 2020
I have blogged about "Providing a GUID function in Azure Resource Manager templates with Azure Functions" that works better than the current template on this repo http://davidjrh.intelequia.com/2017/08/providing-guid-function-in-azure.html 


---
This is a workaround for the problem described at https://feedback.azure.com/forums/281804-azure-resource-manager/suggestions/13067952-provide-guid-function-in-azure-resource-manager-te when you need a random GUID on an Azure Resource Manager template. 
Currently, newGuid function can only be used on resource properties but not as parameters. So, it can not be used to generate random names for Managed Indentity Roles.

The basic idea is to reference this template in your deployment that will generate a "random" GUID for later usage on the same template. 

## Examples

### Getting a guid and using it later

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [ 
    { 
        "apiVersion": "2015-01-01", 
        "name": "MyrandomGuid", 
        "type": "Microsoft.Resources/deployments", 
        "properties": { 
          "mode": "incremental", 
          "templateLink": {
            "uri": "https://raw.githubusercontent.com/tewfikm/azurerm-getrandom/master/getRandom.json",
            "contentVersion": "1.0.0.0"
          }
        } 
    } 
  ],
  "outputs": {
    "result": {
      "type": "string",
      "value": "[reference('MyrandomGuid').outputs.osDiskName.value]"
    }
  }
}
```



```