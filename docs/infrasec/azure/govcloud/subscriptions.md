# Setting Up Azure Government Subscriptions

Setting up subscriptions in Azure Government requires a few specific steps. The instructions below will take you through the process of getting a subscription and managing it within your Azure Government environment.

## Acquiring an Azure Government Subscription

To get started with Azure Government, you first need to obtain a subscription. There are a few ways to do this:

*   **Enterprise Agreement (EA):** If your organization has an existing EA with Microsoft, you can add Azure Government to it.
*   **Cloud Solution Provider (CSP):** You can work with a CSP who is authorized to sell Azure Government.
*   **Pay-As-You-Go:** You can sign up for a pay-as-you-go subscription directly from the Azure Government website.

## Managing Subscriptions

Once you have an Azure Government subscription, you can manage it using the same tools you use for commercial Azure subscriptions, but with different endpoints.

*   **Azure Portal:** The Azure Government portal is located at `https://portal.azure.us`.
*   **Azure CLI:** To use the Azure CLI with Azure Government, you first need to set the cloud to `AzureUSGovernment`:
    ```bash
    az cloud set --name AzureUSGovernment
    ```
    Then you can log in as usual:
    ```bash
    az login
    ```
*   **Azure PowerShell:** To use Azure PowerShell with Azure Government, you need to specify the `AzureUSGovernment` environment when you connect:
    ```powershell
    Connect-AzAccount -Environment AzureUSGovernment
    ```

## Creating Additional Subscriptions

You can create additional Azure Government subscriptions through the Azure portal or by using the Azure CLI or Azure PowerShell. New subscriptions will be placed under the root management group by default. You can then move them to other management groups as needed.

## Terraform

When using Terraform to manage Azure Government resources, you need to configure the Azure provider to use the `usgovernment` environment.

```hcl
provider "azurerm" {
  features {}
  environment = "usgovernment"
}

resource "azurerm_subscription" "example" {
  subscription_name = "My-Azure-Gov-Sub"
  billing_scope_id  = data.azurerm_billing_enrollment_account_scope.example.id
}
```