# Azure Private Link samples

This repository contains templates to deploy sample environments to test and validate **Azure Private Link**. The scenarios are:

**1-custom-service**
Deploys a sample Azure hosted web application, then creates a Private Link to make the application accessible from VNets in Azure. After Private Link is created, the templates deploy a single VNet consumer environment, and allows to create a Private Endpoint with both, auto aproval and manual approval.

**2-hub-and-spoke**
Deploy a sample Azure hosted web application, then creates a Private Link to make the application accessible from VNets in Azure. After Private Link is created, the templates deploy a hub & spoke consumer environment, and creates a Private Endpoint with auto aproval in one of the spoke VNets.

For more information about Azure Private Link, refer to the documentation on: https://docs.microsoft.com/en-us/azure/private-link/
