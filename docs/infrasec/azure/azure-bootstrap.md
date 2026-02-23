# Azure Bootstrap

The purpose of this document is to take you step by step through the
process of bootstrapping a new Azure environment. This is only intended for cases where we are setting up the Azure environment from scratch.

## Naming Conventions

Before starting this process, decide on a naming convention for your
organization. For instance, if you are setting up a new organization
for "spacecats", you will be creating resource groups like
`rg-spacecats-prod-eastus`, `rg-spacecats-dev-eastus`, etc.

## Create a Service Principal

First, you will need to create a service principal that will be used to
authenticate with Azure. This service principal should have the `Owner`
role on the subscription.

```bash
az ad sp create-for-rbac --name "sp-spacecats-terraform" --role "Owner" --scopes "/subscriptions/<subscription-id>"
```

Save the output of this command, as you will need it to configure
Terraform.

## Configure Terraform Backend

Next, you will need to configure a backend for Terraform to store its
state. We recommend using an Azure Storage Account for this.

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "rg-terraform-state"
    storage_account_name = "stterraformstate"
    container_name       = "tfstate"
    key                  = "spacecats.tfstate"
  }
}
```

You will need to create the resource group, storage account, and container
before you can initialize Terraform.

## Configure Terraform Provider

Now, you can configure the Terraform provider for Azure. You will need to
use the credentials for the service principal you created earlier.

```hcl
provider "azurerm" {
  features {}

  subscription_id = "<subscription-id>"
  client_id       = "<client-id>"
  client_secret   = "<client-secret>"
  tenant_id       = "<tenant-id>"
}
```

## Next Steps

Once you have bootstrapped your Azure environment, you can start
deploying resources. We recommend using the enterprise-scale landing
zone architecture as a starting point. See the [Azure
Enterprise-Scale Landing Zone](enterprise-scale-landing-zone.md)
document for more information.
