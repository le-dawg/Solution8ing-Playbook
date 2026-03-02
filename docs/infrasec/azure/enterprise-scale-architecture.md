# Azure Enterprise-Scale Architecture

[Azure enterprise-scale architecture](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/enterprise-scale/) provides a strategic design path and technical state for an organization's Azure environment, enabling effective operationalization of landing zones at scale. It is part of the Microsoft Cloud Adoption Framework (CAF) and offers prescriptive architectural guidance and best practices across critical design areas.

As security best practices prefer subscription boundaries over RBAC policies as a way to limit resource access, Azure enterprise-scale architecture is becoming a standard part of any Azure deployment.

## Solution8 Patterns

As Solution8 begins adopting Azure enterprise-scale architectures for new projects, we are quickly identifying and collecting a number of patterns for organizations. For a more thorough description of the process of bootstrapping a new Azure environment, see the [Bootstrapping an Azure Environment](../bootstrap.md) document, but below is a brief description of the patterns we've adopted.

### Management Groups

- **Keep the hierarchy flat:** A deep and complex management group hierarchy can be difficult to manage. We recommend a flat hierarchy of no more than three to four levels.
- **Organize by function:** We recommend organizing management groups by function, such as `Platform`, `Landing Zones`, and `Sandbox`.
- **Use policies for governance:** Azure Policy is a powerful tool for enforcing standards and compliance across your organization. We recommend using policies at the management group level to enforce tagging, allowed resource types, and other governance standards.

### Subscriptions

- **Democratize subscriptions:** We believe in empowering application teams to manage their own subscriptions. This allows them to move faster and be more agile.
- **Use a landing zone subscription for shared services:** We recommend creating a dedicated subscription for shared services, such as networking, identity, and security. This subscription can be managed by a central platform team.
- **Use a separate subscription for each environment:** We recommend creating a separate subscription for each environment, such as `dev`, `test`, and `prod`. This provides a strong isolation boundary between environments.

### Resource Groups

- **Organize by application or service:** We recommend organizing resource groups by application or service. This makes it easy to see all the resources associated with a particular application or service.
- **Use a consistent naming convention:** We recommend using a consistent naming convention for resource groups, such as `rg-<app-name>-<environment>-<region>`.

## Best Practices

These are the best practices gleaned from online resources and our experiences on various projects.

- **Use Azure Policy to enforce standards:** Azure Policy is a powerful tool for enforcing standards and compliance across your organization. We recommend using policies to enforce tagging, allowed resource types, and other governance standards.
- **Use Azure RBAC to control access:** Azure Role-Based Access Control (RBAC) is a powerful tool for controlling access to Azure resources. We recommend using RBAC to grant users the minimum level of access they need to do their jobs.
- **Do not use a CI/CD pipeline to deploy infrastructure:** Ideally, we'd always use a CI/CD pipeline to deploy infrastructure as code. This helps to ensure that your infrastructure is deployed in a consistent and repeatable way. But this breaks down easily in Azure, when the state of the Control Plane cannot be reliably queried due to, for instance, limited permissions for our users in a client's tenant. Therefore, unless we have tested in conditions that are very close to the real state of the production system, it is better to wait it out. This is especially true when working in highly regulated environments, where change requests take long. So, in a nutshell:
  - Greenfield Project (no Azure systems at all): go right ahead with IaC!
  - Brownfield Project (existing systems in Azure): ask for the highest possible permissions in a separate subscription or directory, even if temporary and eligible. If the client is in a highly regulated environment, avoid this at all costs unless you have an SME on the client team that has at least 3-years of full time experience and the certs in Azure DevOps.

## External links

1. [Azure enterprise-scale architecture](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/enterprise-scale/)
2. [Azure management groups](https://docs.microsoft.com/en-us/azure/governance/management-groups/overview)
3. [Azure subscription decision guide](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/decision-guides/subscriptions/)
