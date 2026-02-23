# Naming Conventions

*Naming* is one of the [hard problems in computing](https://martinfowler.com/bliki/TwoHardThings.html). A well-defined naming convention is a critical component of any enterprise cloud adoption effort. A good naming convention helps you quickly identify a resource's type, its associated workload, its deployment environment, and the Azure region hosting it.

Things are further complicated in Azure because there are different uniqueness constraints in play:

| Uniqueness constraint | Example resources |
| --- | --- |
| Globally unique | App Service, Storage Account, Cosmos DB |
| Resource group unique | Virtual Network, Virtual Machine |
| Subscription unique | Resource Group |

As with all naming schemes (and other stylistic things such as casing and comments) where the client already has a functional naming scheme we should follow that - there are more important issues to deal with. However, for our own work and for projects where we are setting the standard we prefer to use the following.

## Naming components

A good naming convention consists of several components that provide important information about a resource. We recommend the following components:

* **Resource Type:** A unique abbreviation for the Azure resource or service type (e.g., `rg` for resource group, `aks` for AKS cluster, `vm` for virtual machine).
* **Application/Workload/Product:** The name of the application, project, or service the resource belongs to.
* **Environment:** An abbreviation for the environment (e.g., `dev` for development, `test` for testing, `prod` for production, `stg` for staging).
* **Region:** An abbreviation for the Azure region where the resource is deployed (e.g., `eastus`, `westeu`).
* **Instance/Identifier:** A role or identifier to differentiate between multiple instances of the same resource.

A common format might be `<product name>-<type of service abbreviated>-<environment>-<identifier>` or `rg-{app or service name}-{environment}-{region}`.

## Example names for common Azure resources

| Resource type | Recommended abbreviation | Example |
| --- | --- | --- |
| Resource Group | `rg` | `rg-myapp-prod-eastus` |
| Virtual Network | `vnet` | `vnet-myapp-prod-eastus` |
| Subnet | `snet` | `snet-myapp-prod-eastus-001` |
| Virtual Machine | `vm` | `vm-myapp-prod-eastus-001` |
| App Service | `app` | `app-myapp-prod-eastus` |
| Storage Account | `st` | `stmyappprodeastus` |
| Azure Cosmos DB | `cosmos` | `cosmos-myapp-prod-eastus` |
| Azure Kubernetes Service | `aks` | `aks-myapp-prod-eastus` |

## Terraform modules

Typically when building resources and services in terraform, you will follow the naming conventions described above for Azure resources. However, when creating a standalone module, you should use the following convention, which is [required to add the module to the Terraform registry](https://www.terraform.io/docs/registry/modules/publish.html).

`terraform-${provider}-${purpose}` - is the general form

- `$provider` - the terraform provider, e.g. "azurerm", "pagerduty", "github"
- `$purpose` - a simple name describing the role/purpose of the resource, e.g. "microsoft teams-bug-tracker-bot", "webserver", "log-cleaner"

e.g.

- terraform-azurerm-log-cleaner
- terraform-azurerm-cloudtrail-alarms