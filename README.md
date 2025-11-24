⚠️❗ __The engineering playbook is now deployed at [not deployed] using [Docusaurus](https://docusaurus.io/).
We recommend you read it there.
Links and formatting from the old layout may be broken.
Known problems are reported to [GitHub issues](https://github.com/solution8-com/Engineering-Playbook/issues).
Feel free to open a PR if you see something that can be fixed.
Thanks for your patience!__ ❗⚠️

# Engineering Playbook

[![Build](https://github.com/solution8-com/Engineering-Playbook/actions/workflows/pre_commit.yaml/badge.svg)](https://github.com/solution8-com/Engineering-Playbook/actions/workflows/pre_commit.yaml)

[![Deployment](https://github.com/solution8-com/Engineering-Playbook/actions/workflows/deploy.yaml/badge.svg)](https://github.com/solution8-com/Engineering-Playbook/actions/workflows/deploy.yaml)

## Purpose

Within Truss we have much experience of and opinions regarding engineering tools, processes, and practices. The problems and choices that we encounter in our day-to-day practice are rarely new. Having a straightforward way of applying the things we collectively know to the problems we face would be a source of great efficiency for us.

This collection of documents is intended to be simple and searchable, each one containing the essence of Truss opinions on a particular topic. Whilst any Trussel is free to edit these documents, there is some expectation that these are to be curated by the broad engineering community at Truss. To that end, proposed changes should be submitted via a PR and SMEs will be identified to act as curators for particular areas of knowledge.

We build and deploy these docs using [Docusaurus](https://docusaurus.io/), a React-based static site generator, and GitHub Pages.

## Layout

- `/docs/` contains all of our documentation files (in markdown). If you are here to edit or peruse the docs, this is where you want to go.
- `/src/` contains our React components and pages. Currently, this only contains our main page. It will be rare to need to be in this folder.
- `/static/` contains all of our images and other static files. If you want to add a screenshot or other visual to your doc page, you will need to upload it to this folder.
- `/sidebars.js` contains the sidebars for our doc folders. We autogenerate our sidebars in order to minimize how often our JavaScript files need to be updated. It is unlikely that you will need to update this file directly.

## Contents

- [ATOs & Risk Management Framework](./docs/compliance/README.md) - A high level overview of Federal compliance requirements.
- [Developer Tools & Practice](./docs/developing/README.md) - Opinions and resources relating to the tools we use to do our work.
- [Documentation](./docs/documentation/README.md) - How to write effective documentation your users will read.
- [Web Development](./docs/web/README.md) - Languages, frameworks and tools used to develop web applications
- [InfraSec](./docs/infrasec/README.md) - Infrastructure and security are foundational disciplines for building and maintaining stable systems.
- [Leadership](./docs/leadership/README.md) - Guidance and resources around being an Engineering Lead or Manager at Truss.
- [Templates](./docs/templates/README.md) - "Ooh, ooh... I have a thing to add." Here's how to add to this Playbook.
- [Practices](./docs/practices/README.md) - Resources on how the Truss Engineering practices organize.

## Initial Setup (on MacOS)

### Clone the repo

1. Open your terminal/command line.

1. Clone the repo onto your machine and `cd` into it:

   ```
   git clone https://github.com/solution8-com/Solution8ing-Playbook.git && cd Solution8ing-Playbook
   ```

### Install Dependencies

Choose one of the following methods to install the dependencies.

#### Manually

```
brew update
brew install nodenv
brew install pre-commit
pre-commit install
```
### Run the server

```
npm install
npm start
```

The site should load automatically in your browser at
[http://localhost:4000/Solution8ing-Playbook/](http://localhost:4000/Engineering-Playbook/).

If you would like to enable the local search, use the production build instead:

```
npm run build
npm run serve
```

## Deployment

This site is currently deployed using GitHub pages: [https://solution8-com.github.io/Engineering-Playbook/](https://solution8-com.github.io/Engineering-Playbook/). We're using GitHub actions to redeploy whenever changes are merged to the main branch, which includes all commits that are made and saved directly in GitHub.

Be aware that GitHub pages has a _soft_ limit of 10 deploys per hour, and it is possible we could run up against this (read more about the limitations of pages here: [About GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#usage-limits)). It should not have a significant affect on our day-to-day activities, however, and may never become a noticeable issue.

## Licenses

This project uses two licenses. One is for the software components written by
TrussWorks, such as but not limited to plugins, modifications, and scripts to
integrate and deliver the Docusaurus repository. This license the is the
Apache-2.0 license. The other license is for non-software components such as
documentation which mostly reside in the `docs/` directory. This license is the
CCA-4.0 license.

### Apache License, Version 2.0

This license covers the software components of this project and not the
documentation (non-software) components of this project.

This license only references software components written by TrussWorks, LLC to
customize or modify the Docusaurus documentation framework. A list of software
components that fall under this license will be referenced in this file by file
path.

### Creative Commons Attribution 4.0 International Public License

This license covers the documentation (non-software) components of this project
and not the software components of this project.

The non-software components of this project mostly exist in the `docs/`
directory but include other documentation files within the repository. A list of
non-software components outside of the `docs/` directory will be referenced in
this file by file path.

- ./README.md
- ./CODE_OF_CONDUCT.md
- ./CONTRIBUTING.md
