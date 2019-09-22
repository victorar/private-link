# Deploy a sample Azure hosted application and create a Private Link - Private Endpoint to access the application from a hub and spoke network architecture.

These templates deploy a sample Azure hosted web application, then creates a Private Link to make the application accessible from VNets in Azure. After Private Link is created, the templates deploy a hub & spoke consumer environment, and creates a Private Endpoint with auto aproval in one of the spoke VNets.

## Deploying sample templates

You can deploy these sample templates directly through the Azure Portal, Azure Cloud Shell, PowerShell or CLI. To deploy these sample templates with Azure CLI, you can follow these steps (after customizing the parameter files with your desired values):

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
Deploy Consumer Environment (hub & spoke)

3) Create a hub
```bash
cd 3-create-hub
az group create --name plHub --location <azureregion>
az group deployment create --name createHub --resource-group plHub --template-file azuredeploy.json --parameters @azuredeploy.parameters.json
cd ..
```

4) Create Spokes
```bash
cd 4-create-spokes
az group create --name plSpoke --location <azureregion>
az group deployment create --name createSpoke1 --resource-group plSpoke --template-file azuredeploy.json --parameters @azuredeploy1.parameters.json
az group deployment create --name createSpoke2 --resource-group plSpoke --template-file azuredeploy.json --parameters @azuredeploy2.parameters.json
cd ..
```

5) Connect Spokes to Hub via VNet Peering
```bash
cd 5-connect-spokes
az group deployment create --name hub-to-spoke1 --resource-group plHub --template-file azuredeploy.json --parameters @azuredeploy1.parameters.json
az group deployment create --name hub-to-spoke2 --resource-group plHub --template-file azuredeploy.json --parameters @azuredeploy2.parameters.json
az group deployment create --name spoke1-to-hub --resource-group plSpoke --template-file azuredeploy.json --parameters @azuredeploy3.parameters.json
az group deployment create --name spoke2-to-hub --resource-group plSpoke --template-file azuredeploy.json --parameters @azuredeploy4.parameters.json
cd ..
```

6) Create a Private Endpoint with auto-approval
```bash
cd 6-create-private-endpoint
az group deployment create --name createEndpoint --resource-group plSpoke --template-file azuredeploy.json --parameters @azuredeploy.parameters.json
cd ..
```

To validate the setup, you can connect via RDP to the jumpbox deployed in the hub VNet. Once in the jumpbox, connect via RDP to the VMs in spoke1 and spoke2, open a browser, and navigate to the private IP of the Endpoint that was created, for example: http://10.1.0.5
