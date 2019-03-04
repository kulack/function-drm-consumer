# Azure Digi Remote Manager Integration (part 2)

Process Digi Remote Manager Monitor Messages sent from a Digi Remote Manager
monitor and added to a service bus queue by the part 1 sample project
https://github.com/digidotcom/function-drm-monitor.

This function app demonstrates a simple JSON parse and a switch statement
before inserting a DataPoint object into a Cosmos DB collection.

## Quick Deploy to Azure

Use the following buttons, and select the same names for Azure
resource groups that you selected in part one .

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fkulack%2Ffunction-drm-consumer%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fkulack%2Ffunction-drm-consumer%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>

