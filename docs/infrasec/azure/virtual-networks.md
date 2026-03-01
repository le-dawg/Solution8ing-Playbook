# Virtual Network Configuration

## VNet basics

A VNet is a "Virtual Network".
This is a virtual network defined in Azure.
Each VNet is defined in a specified [region](https://azure.microsoft.com/en-us/global-infrastructure/regions/).

The VNet needs a defined CIDR block.
These are private network blocks and need to have enough ip addresses for whatever you plan to deploy.
When in doubt, use a `/16`.

Note that each region has multiple [Availability Zones](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview) and the ip space will be subdivided into subnets across these availabilty zones.

Depending on what you're doing you might just want to use the [terraform-azurerm-vnet module](https://registry.terraform.io/modules/Azure/vnet/azurerm/latest) with the right toggles.

The following are objects associated with VNets that can be configured.

### Subnets

A subnet is a logical subdivision of an IP network.
Generally, you will create a pair of public and private subnets in each availability zone for the region.
A good default CIDR range for each subnet is either a `/22` or `/24`.
You will not want to repeat CIDR ranges across your whole network otherwise your routing might get messy.

Creating additional subnets will create additional complexity in route tables.
If your project requires you to isolate resources from one another, use Network Security Groups.

### Route tables

Route tables define the network routing in the VNet.
You can route specific ip address spaces to separate subnets or to gateways.

### Network Security Groups (NSGs)

NSGs are like firewall rules applied to specific subnets or network interfaces in a VNet.
You can define inbound and outbound network traffic across ports for specific IP addresses.
We prefer to use NSGs over other methods as they can be applied to specific resources in a VNet.

### Application Security Groups (ASGs)

ASGs allow you to group VMs and define network security policies based on those groups.
Because you can define security group access using ASG names, we find it easier to configure intended access to resources than NSGs alone.

### Gateways

Use a Virtual Network Gateway for public subnets.
These allow traffic in and out of the VNet to the public internet.
For all public subnets in a VNet, use a single virtual network gateway for default routes.

Use a NAT Gateway for private subnets.
These only allow traffic out of the VNet to the public internet.
For each private subnet, create a NAT gateway.

### Private Endpoints

Private Endpoints are Azure provided and defined endpoints for Azure services.
You may want to use this to close up your network so you have no public traffic.

### Flow logs

VNet Flow logs allow you to monitor all network traffic in your VNet.

If you plan to use Azure Defender for Cloud you must turn these on.
Otherwise, unless you have a plan to ingest, manage, view, and monitor network flow logs we do not recommend you turn these on.

## Default VNet configuration

Every Azure subscription comes with the ability to create a default VNet.
In general, you will not want to use that VNet.

## Reference links

- [Azure Virtual Network documentation](https://docs.microsoft.com/en-us/azure/virtual-network/)
- [Azure Private Endpoint documentation](https://docs.microsoft.com/en-us/azure/private-link/private-endpoint-overview)
