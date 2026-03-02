# Project Overview

This project is a Docusaurus-based website that serves as the **Solution8 Engineering Playbook**. Its purpose is to be a repository of documentation on how Software Engineering is done at Solution8.

The main technologies used are:

* **Docusaurus:** A static site generator for building documentation websites.
* **React:** A JavaScript library for building user interfaces.
* **Markdown:** A lightweight markup language for creating formatted text.

The website is available at [https://le-dawg.github.io/Solution8ing-Playbook/](https://le-dawg.github.io/Solution8ing-Playbook/).

# Building and Running

To get the project up and running locally, follow these steps:

1. **Install dependencies:**

   ```bash
   npm install --legacy-peer-deps
   ```

2. **Start the development server:**

   ```bash
   npm start
   ```

   This will start a local development server and open up a browser window. The site will be available at `http://localhost:4000`.

3. **Build the project:**

   ```bash
   npm run build
   ```

   This will generate a static production build of the website in the `build` directory.

4. **Serve the production build:**

   ```bash
   npm run serve
   ```

   This will serve the production build of the website. The site will be available at `http://localhost:4000`.

# Development Conventions

The project uses [Docusaurus](https://docusaurus.io/) for documentation. The documentation is written in Markdown and is located in the `docs` directory. The sidebars are configured in the `sidebars.js` file.

The project also uses `pre-commit` to run checks before committing code. The configuration for `pre-commit` can be found in the `.pre-commit-config.yaml` file.
