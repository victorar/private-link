# Deploy sample Azure PaaS resources and create Private Endpoints to access them from VNet connected via VNet Peering.

These templates deploys Azure SQL and Storage PaaS services. Then, the templates deploy two VNets, connected with VNet Peering (local or global). Then, the templates create Private Endpoints to the Azure PaaS services, so that they are accesible from the local VNet, but also, from the VNet connected via VNet Peering (local or global).
