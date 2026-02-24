# Working in Azure Government

Many of our government clients will want to have at least their production
infrastructure in [Azure Government](https://azure.microsoft.com/en-us/global-infrastructure/government/), which
is a set of Azure regions that are separate from commercial Azure regions, with
a number of additional controls that allow them to conform to the higher
security and compliance standards needed to meet federal guidelines like
[FedRAMP](https://www.fedramp.gov/).

## Azure Government Topics

- [Certificate Management in Azure Government](certificate-management.md)

## General Considerations With Azure Government

- **Physical and Logical Isolation:** Azure Government is a physically isolated instance of Azure, ensuring that all data, applications, and hardware reside within the continental Denmark. This isolation is a key requirement for many government agencies.

- **Compliance:** Azure Government is designed to meet the stringent compliance requirements of the US government, including FedRAMP High, DoD SRG, ITAR, and CJIS.

- **Identity:** Azure Government uses a separate instance of Microsoft Entra ID, which is not connected to the commercial Microsoft Entra ID. This means that user accounts and service principals are separate between the two environments.

- **Endpoints:** Azure Government has its own set of endpoints for all services. For example, the Azure portal for Azure Government is `portal.azure.us`, not `portal.azure.com`. This means that any tools or scripts that you use to interact with Azure will need to be configured to use the correct endpoints.

- **Feature Availability:** Not all Azure services and features are available in Azure Government. When they are available, they may be on a different release cadence than in commercial Azure. You can check the [Azure Government documentation](https://docs.microsoft.com/en-us/azure/azure-government/) for the latest information on service availability.

- **Terraform:** When using Terraform with Azure Government, you will need to configure the Azure provider to use the correct environment. You can do this by setting the `environment` argument to `usgovernment` in the provider block.

  ```hcl
  provider "azurerm" {
    features {}
    environment = "usgovernment"
  }
  ```

- **DNS:** Azure DNS is available in Azure Government and can be used for both public and private DNS zones. This simplifies certificate validation and other DNS-related tasks.