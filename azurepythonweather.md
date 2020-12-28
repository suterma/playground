# A simple Azure weather station project (for learning purposes)

## Install python 3 on a Raspberri Pi zero

    sudo apt install python3

By default, the most recent version available is 3.5.3 (as of today)

## Use the azure IoT SDK to post data

    pip3 install azure-iot-device


 - https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples
 
## The JSON format to use

Currently used: 

    PS /home/marcel> az iot hub monitor-events --hub-name weathertestiothub
    This extension 'azure-cli-iot-ext' is deprecated and scheduled for removal. Please remove and add 'azure-iot' instead.
    Starting event monitor, use ctrl-c to stop...
    {
    "event": {
            "origin": "raspberrypi-zero-wh",
            "payload": "{ \"timestamp\": \"1608643863720\", \"locationDescription\": \"Attic\", \"temperature\": \"21.941\", \"relhumidity\": \"71.602\" }"
        }
    }
    

This seems not to work, find resources....
Is it because of the escapes?
This is about Gen1 only: https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-send-events#supported-json-shapes

:todo: Continue here

## Use the Azure IoT Hub with EventGrid to route data

## Publish charts tbd

Nodejs client app, with IoTHub: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps

## Deploy from GitHub to Azure Web App
See https://docs.microsoft.com/en-us/azure/app-service/scripts/cli-deploy-github#sample-script

    #!/bin/bash
    
    # Replace the following URL with a public GitHub repo URL
    gitrepo=https://github.com/suterma/web-apps-node-iot-hub-data-visualization
    webappname=mywebapp$RANDOM
    
    # Create a resource group.
    az group create --location westeurope --name githubdeploydemo
    
    # Create an App Service plan in `FREE` tier.
    az appservice plan create --name $webappname --resource-group githubdeploydemo --sku FREE
    
    # Create a web app.
    az webapp create --name $webappname --resource-group githubdeploydemo --plan $webappname
    
    # Deploy code from a public GitHub repository. 
    az webapp deployment source config --name $webappname --resource-group githubdeploydemo \
    --repo-url $gitrepo --branch master --manual-integration
    
    # Copy the result of the following command into a browser to see the web app.
    echo http://$webappname.azurewebsites.net


Assuming, an App Service Plan already exists:
    
    az webapp create -n qrysweather -g testrg -p weathertest --runtime "node|10.6" --deployment-local-git
