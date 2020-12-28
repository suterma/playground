# A simple Azure weather station project (for learning purposes)

## Publish charts tbd

See https://github.com/suterma/web-apps-node-iot-hub-data-visualization

Nodejs client app, with IoTHub: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps

## Deploy from GitHub to Azure Web App
See https://docs.microsoft.com/en-us/azure/app-service/scripts/cli-deploy-github#sample-script

:todo: Continue here: Results in Status 500 errors, How to resolve
https://portal.azure.com/#@marcelsuternowhow.onmicrosoft.com/resource/subscriptions/8f55d6a7-1c33-43bd-aa65-cb6de4073a27/resourceGroups/githubdeploydemo/providers/Microsoft.Web/sites/mywebapp28304/appServices

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
