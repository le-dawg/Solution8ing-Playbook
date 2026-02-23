# Project Teardown Guide

When a project reaches the end of its lifetime and the client doesn't
want to continue it, we need to teardown all the infrastructure we've
created for the project -- sometimes on relatively short notice. This
guide is intended to provide a step-by-step procedure for tearing down
infrastructure we've created for a project and highlight any potential
sources of trouble.

## Terraform and Azure

Your first task should be shutting down all the Azure infrastructure
you've built for your project. When you do this, you'll need to proceed
in basically the reverse order you created all the resources. Here are
some guidelines when tearing down Terraform workspaces:

- Tear down resource groups that don't hold resources for organization-wide
  purposes first -- leave your platform, identity, and management group resources for last.

- If you were using a CI/CD pipeline to maintain a workspace, and you have this
  component in your `terraform.tf` `backend` configuration:

  ```hcl
  backend "azurerm" {
    ...
  }
  ```

  Then before you begin tearing down that workspace, you need to remove
  this line, run a `terraform apply`, and then do your `terraform destroy`.
  If you do not do this, then your `destroy` will fail and the state will
  become corrupted because you will destroy the storage account Terraform is using
  to perform the destroy. Doing the `terraform apply` allows Terraform to
  cleanly change the backend configuration first.

- It's easiest to wait until all resources in all your resource groups are
  destroyed to begin deleting resource groups; this will remove all the
  resource locks and let you remove resource groups from the subscription
  as you go.

- In order to completely close a subscription, you'll need to cancel it in the Azure portal.

## SSL certificates

For most projects, we'll hopefully be able to use Azure Key Vault certificates,
and those will get torn down with our Terraform teardown above. However,
if we've bought additional SSL certificates through another vendor, such
as SSLMate, we should revoke those certificates and close that account
as well.

## CI/CD

Assuming you've used a CI/CD pipeline on the project,
you should be able to just terminate your support plan as soon
as you don't need to do anymore deploys or validate code.

## GitHub

Once you've torn down your Azure infrastructure and CI/CD, you can
shutdown your GitHub organization for the project. Here are the
guidelines for taking down your GitHub organization:

- Make sure the client has been able to make an archive of all
  repositories before you do anything else. Code we build for a client
  belongs to them, so we need to ensure they are able to keep a copy.
- If we are allowed (this varies based on the terms of the contract), we
  should make sure we keep an archive of any code repositories we want
  to retain.
- Delete any GitHub users that were created for CI/CD automation or other
  purposes. These are commonly created to allow CI/CD to access private
  repositories.
- Once the above steps have been completed, you can go into the settings
  for the GitHub organization and close it down. *This will delete all
  repositories in the organization*, so make sure you're ready for that
  before you do this.

## 1Password

*Deleting your 1Password account and vault for the project should be the
very last thing you do.* This is (hopefully) where you've kept all your
credentials for services, software, and accounts that this project uses,
so getting rid of this makes it *extremely* difficult to clean up anything
else you were using for your project. We recommend that you carefully
review the previous steps of this guide and look at the credentials kept
in 1Password to ensure that you've closed down everything else that you
are relying on 1Password for.