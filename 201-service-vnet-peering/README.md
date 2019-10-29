# Deploy a sample Azure hosted application and create a Private Link - Private Endpoint to access them from VNet connected via VNet Peering.

These templates deploy a sample Azure hosted web application, then creates a Private Link to make the application accessible from VNets in Azure. After Private Link is created, the templates deploy two VNets in Azure connected with VNet Peering (local or global). Then, the templates create a Private Endpoint to the Azure hosted web application, so that it is accesible from the local VNet, but also, from the VNet connected via VNet Peering (local or global).

