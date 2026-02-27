# Project Bootstrap Guide

When we're starting up a new project, often there is a period of time
where the design and research team is doing their initial work, we don't
have any infrastructure set up, and engineers want to do some initial
experimentation. This guide is intended as a resource for infrastructure
engineers bootstrapping a new project from scratch. Not all projects will
need all of the elements here; for instance, if you're working in an
existing organization's Azure subscriptions or git repos, then you won't likely
need to set up your own.

<!-- toc -->

- [GitHub and Git Repos](#github-and-git-repos)
- [1Password](#1password)
- [Azure Organization and Subscriptions](#azure-organization-and-subscriptions)
  - [Azure Government](#azure-government)
  - [Placeholder service modules](#placeholder-service-modules)
- [CI/CD Pipeline](#cicd-pipeline)

<!-- Regenerate with "pre-commit run -a markdown-toc" -->

<!-- tocstop -->

## GitHub and Git Repos

The first thing you will likely need to do for your project is create
a number of GitHub repos and possibly a new GitHub organization. This
is the prerequisite for nearly everything else you need to do to set
up your infrastructure, so doing it as early as possible is a good idea.

- If your client does not have an organization for you to do this in,
  or if they would prefer you create a new one, then you will need to
  create a new GitHub organization. GitHub has
  [docs](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/creating-a-new-organization-from-scratch)
  that explain this process; you will probably want the "GitHub Team"
  product, and use the Solution8 admin credit card to handle this cost. It's
  also a good idea to make sure the leads for your project are owners,
  and probably your contact at the client organization as well. This
  will allow them to easily add or remove new members if necessary.
- Once you have a GitHub organization, you will need to create a number
  of repositories:
  - `myproject-infra` - This is a repo which you will use to hold the
    Terraform code used to configure your project's Azure (or other cloud
    service) subscription(s).
  - application repo(s) - You should talk to the AppEng lead to see how
    they would like to organize the application and create repos they
    can use early on. Some projects may want a single repo
    for all the application code, but other projects may want separate
    repos for the frontend and backend, for instance. Neither of these
    is inherently correct, so consult with the AppEng lead (if you have
    one) to make this decision.
   - `myproject-infra-gov` - If your project is going to need to have
     resources in Azure Government, as many of ours do, you will need another repo
     for the Terraform used for that. This is for two reasons; one, we
     generally want tighter permissions on who has access to this repo, and
     second, separating commercial and government infrastructure reduces risk by limiting blast radius and keeping compliance boundaries distinct.
- If at all possible, you should be using Terraform to maintain the users,
  teams, and repos for your project. The catch being that you'll need your
  `myproject-infra` repo first.

## 1Password

It is highly likely that you will want to get a 1Password team for your
project. This will allow you to store things like passwords and MFA codes
for Azure accounts, GitHub "robot" users, and anything else that you need
to give members of the team access to.

You can create a new account by going to <https://1password.com> and
clicking on "sign in" in the upper right hand corner, and then instead
of clicking on an existing account, click on "create a new account" at
the bottom of the page, then click on "for my team" on the next page,
and follow the creation dialogue. Use the Solution8 admin card for the
charges.

You should only need to add people who actually need to access these
credentials for the project to the team (since there's a charge per-user).
This generally means all members of the infra team, the engineering lead,
and the project lead -- but you can always add more people later if
necessary.

Like the GitHub organization/repositories, this should probably be done
as early in the process as possible, since you will want to store
credentials for the other accounts you are creating.

## Azure Organization and Subscriptions

Once you have a Git repo and 1Password set up, you will want to set up
your Azure organization and/or subscriptions. Even if you don't have an actual
application to stand up yet, you should get this bootstrapped so that
things are ready to go.

If you are starting from scratch, you will need to create a new Azure subscription
by going to <https://portal.azure.com> and creating a subscription, using
the Solution8 admin card as the billing information. This account will be your
root subscription that will start your
[Azure Enterprise-Scale architecture](./azure/enterprise-scale-architecture.md). Once that is created, you
should bootstrap Terraform in it and set up the management groups and
subscription structure.

Things you will want to set up:

- Azure Log Analytics workspace for centralized logging
- Microsoft Entra ID users for infrastructure users (infra users only)
- Azure Policy for the organization
- Azure Defender for Cloud for the organization

Once that is done, you'll also need to set up some other subscriptions using
Terraform:

- Management subscription for governance and security
- Connectivity subscription for network infrastructure
- Landing zone subscriptions for workloads (dev, staging, prod)

You may want to set up others as well, but these are probably the ones you want as soon as possible.


### Placeholder service modules

Once the AppEng lead has identified, at a high level, the services they
will be deploying -- such as a frontend, backend, and database -- you
should start building Terraform modules to deploy these components. Even
if, at this point, they are largely placeholders, you should start
figuring out what you'll need to get these working -- things like Azure
App Service or Azure Container Instances, Azure SQL or Cosmos DB, Azure
Load Balancers, Virtual Networks, security groups, managed identities, etc.

Even if there isn't much to actually put there, and the service will be
dormant, working this out *before* you need to start demoing things for
the client will help you work out any kinks and help developers get a
tight feedback loop once they start working on the application in earnest.

## CI/CD Pipeline

Setting up your CI/CD pipeline early is another piece of automation that
will pay off in the long run. Doing so means that developers will get used
to the idea that their changes will be put, at the very least, into a real
running environment every time they push, which means that building in
tests and operability are not optional components or things to be added
later.

Solution8 tends to use either CircleCI or GitHub Actions for our projects;
GitHub Actions are newer and as a result our patterns for them are not as
well-developed, however they have a lower barrier to entry and you can
use them on a limited basis for free even with private repos. It may also
easier to use GitHub Actions in environments where clients are cool on
using a SaaS product like CircleCI (since they are already likely using
GitHub). Alternatively, you may need to use a CI/CD pipeline that the
client already uses.

Regardless of which you pick, you should start wiring up the application
repo(s) to your pipeline early, and making sure there is an automated
deployment into the dev environment at the very least as close to the
beginning of the project as possible. This means that your pipeline
should at least do the following:

- run pre-commit checks on the repo for each PR
- run tests (and possibly output code coverage metrics as well)
- build a docker image and push it to Azure Container Registry
- attempt to deploy the application, usually by updating the container
  instance or app service, and if at all possible, ensure that the correct version
  comes up and stays up, rolling back if it does not
