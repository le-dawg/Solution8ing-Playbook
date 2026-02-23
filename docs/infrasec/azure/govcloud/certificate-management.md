# Certificate Management in Azure Government

Azure Key Vault is our preferred way to handle certificates in Azure whenever possible. In Azure Government, the process of creating and validating certificates is streamlined and can be fully automated.

## The solution: Automated validation with Azure DNS and Key Vault

Azure Key Vault can be configured to automatically handle the entire lifecycle of a certificate, including creation, validation, and renewal. When you request a certificate from Key Vault, it can automatically create the necessary DNS validation records in an Azure DNS zone.

Here's a high-level overview of the process:

1.  **Create an Azure Key Vault:** If you don't have one already, create an Azure Key Vault in your Azure Government subscription.
2.  **Create an Azure DNS Zone:** If you don't have one already, create a public Azure DNS zone for your domain.
3.  **Grant Key Vault access to the DNS Zone:** Grant the Key Vault service principal the "DNS Zone Contributor" role on your Azure DNS zone. This allows Key Vault to create the necessary DNS records for validation.
4.  **Request a certificate from Key Vault:** You can request a certificate directly from Key Vault. Key Vault will automatically create the DNS TXT record to validate domain ownership.
5.  **Use the certificate:** Once the certificate is issued, you can use it in your applications, for example, by binding it to an App Service.

Here's some sample Terraform code to demonstrate the process:

```hcl
resource "azurerm_resource_group" "rg" {
  name     = "rg-myapp-prod-eastus"
  location = "eastus"
}

resource "azurerm_dns_zone" "dns_zone" {
  name                = "myapp.example.com"
  resource_group_name = azurerm_resource_group.rg.name
}

resource "azurerm_key_vault" "key_vault" {
  name                = "kv-myapp-prod-eastus"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  tenant_id           = data.azurerm_client_config.current.tenant_id
  sku_name            = "premium"
}

resource "azurerm_key_vault_certificate" "cert" {
  name         = "myapp-example-com"
  key_vault_id = azurerm_key_vault.key_vault.id

  certificate_policy {
    issuer_parameters {
      name = "Self"
    }

    key_properties {
      exportable = true
      key_size   = 2048
      key_type   = "RSA"
      reuse_key  = true
    }

    lifetime_action {
      action {
        action_type = "AutoRenew"
      }

      trigger {
        days_before_expiry = 30
      }
    }

    secret_properties {
      content_type = "application/x-pkcs12"
    }

    x509_certificate_properties {
      subject            = "CN=myapp.example.com"
      validity_in_months = 12

      subject_alternative_names {
        dns_names = ["myapp.example.com"]
      }
    }
  }
}
```

With this configuration, Azure Key Vault will handle the certificate creation and renewal process automatically. There is no need for manual intervention or step-by-step creation processes.