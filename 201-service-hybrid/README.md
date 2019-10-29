# Deploy sample Azure hosted application and create Private Endpoints to access them from on-premises.

These templates deploy a sample Azure hosted web application, then creates a Private Link to make the application accessible from VNets in Azure and from on-premises via VPN or ExpressRoute. After Private Link is created, the templates deploy a single VNet consumer environment, and creates a Private Endpoint to the Private Link Service. Finally, the templates create a VPN or ExpressRoute connection to on-premises to test connectivity to the Private Endpoint from on-premises.
