# Deploy a sample Azure hosted application and create a Private Link - Private Endpoint to access the application from a virtual network.

These templates deploy a sample Azure hosted web application, then creates a Private Link to make the application accessible from VNets in Azure. After Private Link is created, the templates deploy a single VNet consumer environment, and allows to create a Private Endpoint with both, auto aproval and manual approval.

## Deploying sample templates

You can deploy these sample templates directly through the Azure Portal, Azure Cloud Shell, PowerShell or CLI. 

To deploy these sample templates with Azure CLI, you can follow these steps (after customizing the parameter files with your desired values):

0) Login to your Azure subscription
```bash
az login
az account set --subscription <sub-id>
```
Deploy Provider Environment

1) Deploy a sample Azure hosted web application
```bash
cd 1-create-custom-service
az group create --name privateLinkService --location <azureregion>
az group deployment create --name deployWebApp --resource-group privateLinkService --template-file azuredeploy.json --parameters @azuredeploy.parameters.json
cd ..
```

2) Create a Private Link for the sample Azure hosted web application
```bash
cd 2-create-private-link-service
az group deployment create --name createPrivateLink --resource-group privateLinkService --template-file azuredeploy.json --parameters @azuredeploy.parameters.json
cd ..
```
Deploy Consumer Environment (single VNet)

3) Create consumer environment with a single VNet
```bash
cd 3-create-consumer-environment
az group create --name privateLinkConsumer --location <azureregion>
az group deployment create --name createConsumerEnvironment --resource-group privateLinkConsumer --template-file azuredeploy.json --parameters @azuredeploy.parameters.json
cd ..
```

4.1) Create a Private Endpoint with auto-approval
```bash
cd 4-1-create-private-endpoint-auto-approval
az group deployment create --name createAutoEndpoint --resource-group privateLinkConsumer --template-file azuredeploy.json --parameters @azuredeploy.parameters.json
cd ..
```

4.2) Create a Private Endpoint with manual-approval
```bash
cd 4-2-create-private-endpoint-manual-approval
az group deployment create --name createManualEndpoint --resource-group privateLinkConsumer --template-file azuredeploy.json --parameters @azuredeploy.parameters.json
cd ..
```

To approve the Private Endpoint with manual approval, just go to the Private Link service created on step 2), under Settings click on Private endpoint connections. Select the connection that is waiting for approval, and click on Approve.

To validate the setup, you can connect to the consumer VM using Bastion host, open a browser, and navigate to the private IP of the Endpoints that were created, for example: http://10.0.0.5
