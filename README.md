# Azure Digi Remote Manager Integration (part 2)

Process Digi Remote Manager Monitor Messages that were added to a service bus queue by the part 1 sample project at
https://github.com/kulack/function-drm-monitor.

This function app demonstrates a simple JSON parse and a switch statement
before inserting a DataPoint object into a Cosmos DB collection.

You can easily extend it to other event types and other data connectors.

## Quick Deploy to Azure

Use the following buttons, and select the same names for Azure
resource groups that you selected in the part 1 project.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fkulack%2Ffunction-drm-consumer%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fkulack%2Ffunction-drm-consumer%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>

## See part 1

Part 1 is a Function App that accepts the messages sent from Digi Remote Manager and adds them to the service bus for this project to process.

Part 1 of this sample can be found at https://github.com/kulack/function-drm-monitor.

## Digi Remote Manager Instructions

After deploying both applications to Azure, navigate to the new Function App (sometimes called an App Service) using Azure Portal.

Select the drm-monitor function and take note of the API endpoint and the function key code.

The URL with the keycode looks something like this:

```
    https://function-drm-monitorXXXX.azurewebsites.net/api/drm-monitor?code=abcdefghijklmnopqrstuvwxyz
```


Next, create a json monitor in your Digi Remote Manager account using the API endpoint and the `x-functions-key` header specifying the function key code.

Navigate to Digi Remote Manager API Explorer, and select:
*  Examples -> Cloud Integrations -> Send Events to Azure

The following XML is displayed and will define a new monitor:

* Replace the contents of `monTransportUrl`
with your API endpoint and the contents of `monTransportHeaders` with your
function key code.

* Replace the contents of `monTopic` with the Digi Remote Manager event types
  that your solution needs.

* Adjust the batch size and duration if required

* Note that this monitor does NOT replay events to the target after system downtime.

  ```
    <Monitor>
      <monTopic>alert_status,devices</monTopic>
      <monTransportType>http</monTransportType>
      <monTransportUrl>https://function-drm-monitorXXXX.azurewebsites.net/api/drm-monitor</monTransportUrl>
      <monFormatType>json</monFormatType>
      <monBatchSize>1000</monBatchSize>
      <monBatchDuration>15</monBatchDuration>
      <monDescription>Push Monitor for Azure integration</monDescription>
      <monTransportHeaders>x-functions-key: YOUR-FUNCTION-KEY-CODE</monTransportHeaders>
      <monTransportMethod>POST</monTransportMethod>
      <monAutoReplayOnConnect>false</monAutoReplayOnConnect>
    </Monitor>
  ```

* In the Digi Remote Manager API Explorer submit the request.

  * Alternatively, use the XML definition of the monitor as the body for the /ws/Monitor API in Digi Remote Manager using the Digi Remote Manager API Explorer or your favorite web API client.

  * For example, in Linux or Mac OS, you can use curl to create a DRM Monitor using
something similar to this (after putting the XML shown above in a file called MonitorPayload.xml)

    ```
    curl -X POST --data @MonitorPayload.xml -u user:password https://remotemanager.digi.com/ws/Monitor
    ```

## Application settings

Specify these values during deployment:

* The name of the logic app to be created. The default name is `function-drm-consumer0000`
* The new or existing resource group and location for created resources.
* The frequency and interval at which to receive messages (default every 1 minutes).
* The Cosmos DB connection information (a database named "timeseries" with a collection named "DataPoint" and partition key of "/id" is required)
* The Service Bus neame (a queue named "drmposted" that receives messages from the Part 1 code is required)

