# Convert an existing Azure hosted application to a Private Link Service

This template converts an existing Azure hosted application to a Private Link Service. It assumes you already have an Azure hosted application behind a Standard Load Balancer (either public or internal). Optionally, you can deploy a sample web app behind a public Standard Load Balancer to test how to convert it to a Private Link Service.

![Private Link Service](https://github.com/victorar/private-link/blob/master/media/Converting-to-PrivateLink.png)

## Deploying sample templates

You can deploy these sample templates directly through the Azure Portal, Azure Cloud Shell, PowerShell or CLI. 

To deploy these sample templates with Azure CLI, you can follow these steps (after customizing the parameter files with your desired values):

0) Login to your Azure subscription
```bash
az login
az account set --subscription <sub-id>
```

1) **(Optional)** Deploy a sample Azure hosted web application in case you don't have an existing application behind a public Standard Load Balancer
```bash
cd 1-create-sample-webapp
az group create --name azureHostedApp --location <azureregion>
az group deployment create --name deployWebApp --resource-group azureHostedApp --template-file azuredeploy.json --parameters @azuredeploy.parameters.json
cd ..
```

2) Create a Private Link for your Azure hosted application
```bash
cd 2-create-private-link-service
az group deployment create --name createPrivateLink --resource-group azureHostedApp --template-file azuredeploy.json --parameters @azuredeploy.parameters.json
cd ..
```

Once the createPrivateLink deployment completes successfully, you can go to the Azure Portal, navigate to the resource group used for this deployment, and you will notice the Private Link Service that was created for your Azure hosted application that is behind a Standard Load Balancer (public or internal).

Now, you can provide the Private Link Service Id or Alias to your consumers, so that they connect to your service via a Private Endpoint.
