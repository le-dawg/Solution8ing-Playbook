# `terraform import`

If you're adding Terraform to an environment where Terraform was not being
used, often you'll need to import existing resources into your Terraform
deployment so they can be managed with your other Terraformed resources.
The [Terraform documentation](https://www.terraform.io/docs/import/index.html)
describes how to do this and will give you a detailed breakdown of the
command; this page provides a tangible example and tries to explain potential
pitfalls.

## A Word of Warning

Remember when importing resources that you may need to import multiple
resources to get everything that might seem like a single component in
Azure. If you import an Azure Blob Storage, that does *not* import its analytics
configuration, for instance, which is a completely different Terraform
resource.

## Importing a resource as a raw Terraform resource

In this case, we're going to be importing an existing Azure Blob Storage account
named `mystorageacct001` into our Terraform deployment as a Terraform
`azurerm_storage_account` resource.

1. Write the Terraform code that will describe this resource in your
   infrastructure. For our example, we would add this to our Terraform
   infrastructure:

   ```hcl
   resource "azurerm_storage_account" "my_storage_account" {
     name                     = "mystorageacct001"
     account_tier             = "Standard"
     account_replication_type = "LRS"

     tags = {
       Name        = "My Bucket"
       Environment = "Dev"
     }
   }
   ```

1. Run `terraform import` in the namespace that file is in to import the
   resource. The `import` command requires two arguments; the `ADDRESS` and
   the `ID`. The `ADDRESS` is the terraform resource we just described, so
   `azurerm_storage_account.my_storage_account` in the example above. The `ID` depends on
   the resource we're importing; you can find out at the bottom of the
   documentation for that resource in the Terraform docs. You can see in the
   docs for
   [`azurerm_storage_account`](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/storage_account#import)
   that for that resource, it's just the storage account resource ID. So we would run this
   command:

   ```bash
   $ terraform import azurerm_storage_account.my_storage_account /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/my-rg/providers/Microsoft.Storage/storageAccounts/mystorageacct001
   azurerm_storage_account.my_storage_account: Importing from ID "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/my-rg/providers/Microsoft.Storage/storageAccounts/mystorageacct001"...
   azurerm_storage_account.my_storage_account: Import prepared!
     Prepared azurerm_storage_account for import
   azurerm_storage_account.my_storage_account: Refreshing state... [id=/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/my-rg/providers/Microsoft.Storage/storageAccounts/mystorageacct001]

   Import successful!

   The resources that were imported are shown above. These resources are now in
   your Terraform state and will henceforth be managed by Terraform.

   Releasing state lock. This may take a few moments...
   ```

1. Our resource has been imported. Now if we run a `terraform plan`, we
   can see that Terraform has added the bucket to the state:

   ```bash
   $ terraform plan

   ...

   An execution plan has been generated and is shown below.
   Resource actions are indicated with the following symbols:
     ~ update in-place

   Terraform will perform the following actions:

     # azurerm_storage_account.my_storage_account will be updated in-place
     ~ resource "azurerm_storage_account" "my_storage_account" {
         + account_kind             = "StorageV2"
           id                       = "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/my-rg/providers/Microsoft.Storage/storageAccounts/mystorageacct001"
           name                     = "mystorageacct001"
           primary_blob_endpoint    = "https://mystorageacct001.blob.core.windows.net/"
           primary_web_endpoint     = "https://mystorageacct001.z13.web.core.windows.net/"
           location                 = "westus2"
         ~ tags                        = {
             + "Environment" = "Dev"
               "Name"        = "My Bucket"
           }

        }

   Plan: 0 to add, 1 to change, 0 to destroy.
   ```

   Note that it is only changing an existing resource. Also note that the
   `Name` tag already existed on the existing storage account, so that will not be
   changed, but that we're adding the `Environment` tag to the storage account. To
   make these changes, we'd run a `terraform apply` and then we should have
   a clean plan after that. We can then use this resource as if it had
   always been in Terraform.

## Importing a resource as a component of a module

It's also possible to import an existing resource as a component of a
Terraform module, we just have to change our Terraform code and the
`ADDRESS` component of the `terraform import` command. So if we wanted
to use the Solution8 `terraform-azurerm-storage-private-container` module for our storage account
instead of the raw resource, we'd have Terraform code like this:

```hcl
module "my_storage_account" {
  source         = "Solution8works/storage-private-container/azurerm"
  storage_account_name = "mystorageacct001"
  logging_storage_account = "myloggingacct001"

  tags = {
    Name        = "My Bucket"
    Environment = "Dev"
  }
}
```

Then we would run this command:

```bash
$ terraform import module.my_storage_account.azurerm_storage_account.private_storage_account /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/my-rg/providers/Microsoft.Storage/storageAccounts/mystorageacct001
```

We're getting that `ADDRESS` by looking at the code of the module and
seeing where it is actually defining that resource that we want to import;
in this case, it's the
[`main.tf`](https://github.com/Solution8works/terraform-azurerm-storage-private-container/blob/master/main.tf)
file, on this line:

```hcl
resource "azurerm_storage_account" "private_storage_account" {
  name                     = local.storage_account_name
  account_tier             = "Standard"

...
```

## Resources

- [Importing Infrastructure](https://www.terraform.io/docs/import/index.html)
- [Importing Existing Infrastructure into Terraform – Step by Step](https://spacelift.io/blog/importing-exisiting-infrastructure-into-terraform)
