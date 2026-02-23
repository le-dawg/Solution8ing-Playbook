# Azure Enterprise-Scale Landing Zone

An Azure enterprise-scale landing zone is a strategic design path and target technical state for an organization's Azure environment, based on Microsoft's Cloud Adoption Framework (CAF). It provides prescriptive architectural guidance and best practices for setting up a scalable, secure, and well-governed Azure environment.

## Key Design Areas

The enterprise-scale architecture provides detailed recommendations across eight critical design areas:

*   Enterprise Agreement (EA) enrollment and Azure Active Directory tenants.
*   Identity and access management (IAM).
*   Management group and subscription organization.
*   Network topology and connectivity.
*   Management and monitoring.
*   Business continuity and disaster recovery (BCDR).
*   Security, governance, and compliance.
*   Platform automation and DevOps.

## Solution8's Approach

At Solution8, we have adopted the enterprise-scale landing zone architecture as our standard for all new Azure environments. Our implementation focuses on the following key principles:

*   **Subscription democratization:** Empowering application teams to manage their own subscriptions while maintaining central governance.
*   **Policy-driven governance:** Enforcing compliance and security through Azure Policy.
*   **Single control and management plane:** Centralizing management and operations.
*   **Application-centric and archetype neutral:** Focusing on application needs and supporting various architectural patterns.
*   **Align Azure-native design and roadmap:** Utilizing Azure's native capabilities and aligning with its future direction.

## Implementation

Our implementation of the enterprise-scale landing zone architecture is based on the following components:

*   **Management Groups:** We use a flat management group hierarchy to organize subscriptions by function, such as `Platform`, `Landing Zones`, and `Sandbox`.
*   **Subscriptions:** We use a dedicated subscription for shared services, such as networking, identity, and security. We also create a separate subscription for each environment, such as `dev`, `test`, and `prod`.
*   **Resource Groups:** We organize resource groups by application or service and use a consistent naming convention.
*   **Azure Policy:** We use Azure Policy to enforce tagging, allowed resource types, and other governance standards.
*   **Azure RBAC:** We use Azure Role-Based Access Control (RBAC) to grant users the minimum level of access they need to do their jobs.
*   **CI/CD:** We use a CI/CD pipeline to deploy infrastructure as code.

For a step-by-step guide on how to bootstrap a new Azure environment, see the [Azure Bootstrap](azure-bootstrap.md) document.
