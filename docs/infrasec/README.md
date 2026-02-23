---
sidebar_position: 3
---

# InfraSec Practice

Infrastructure and security engineering (InfraSec) is the practice of
building secure, robust systems that are foundational to having reliable
applications and services. While infrastructure as code is a core area
for this practice, it also involves system design, [incident
response](../incident-response/README.md), and a number of other fields.

## Theory

- [InfraSec Practice Charter](charter.md) - How we think about the Solution8 InfraSec Practice.
- [Good Infrastructure - A Philosophy](good-infra.md) — How we think about building good infrastructure.
- :lock: [InfraSec "Book" Club - Talk and Article Suggestions](https://docs.google.com/document/d/1X0GDtCMrPnl_Zdpo0qEtaxUW4SocxnZairCM9JQAcyo/edit) — Talks and articles we :sparkling_heart:.

## Practice

- [Azure](azure/README.md) — Our primary cloud provider.
- [Terraform](terraform/README.md) — Our primary infrastructure as code (IaC) tool.
- [CI/CD at Solution8](./ci_cd.md) — Guiding stars for building CI/CD, the Solution8 way.
- [Ansible](ansible/README.md) — For when we have to build non-container based images.

### Education recommendations

Things to help you level up your skills.

- [Professional development recommendations](pro-dev.md)

### Useful Repo Templates

To get you up and running faster, we have created a few template repos. Please feel free to submit PRs and help us stay up to date!

- [Terraform Module](https://github.com/le-dawg/terraform-module-template)
- [Docker container](https://github.com/le-dawg/docker-template)
- [Golang CLI tool](https://github.com/le-dawg/golang-cli-template)

### Tutorials

#### Azure

- [Setting Up Your Azure User](azure/setting-up-azure-user.md) — How to set up your Azure user in the Solution8 internal infrastructure.
- [Azure Bootstrap](azure/azure-bootstrap.md) — Getting started with Azure.

#### CI/CD

- [Honeycomb CircleCi Metrics](tutorials/circle-ci-honeycomb-integrations.md) - How to add Honeycomb to CircleCi for build metrics.

#### Security

- [One-Time Passwords](tutorials/one-time-passwords.md) — How to set up one-time passwords for GitHub with 1Password.

### Other topics

- [Alert Providers](alert-providers.md)
- [Project Bootstrap Guide](bootstrap.md)
- [Project Teardown Guide](teardown.md)
- [SSL Certificates](certs.md)
