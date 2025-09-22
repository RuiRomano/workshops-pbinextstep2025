# Power BI for Developers: PBIP and CI/CD Unleashed

Hands-on deep dive into Power BI developer mode and the powerful capabilities unlocked by Power BI Project files (PBIP). In this workshop, you’ll explore TMDL and PBIR file formats, learning how they open the door to structured, source-controlled development workflows.

We’ll show you how to accelerate your Power BI development using TMDL and PBIR – enhanced with AI-assisted tooling – to boost productivity and consistency.

You’ll also learn how to bring modern DevOps practices into your BI projects, from seamless CI/CD pipelines to automated deployments, using both built-in features and innovative community-driven solutions.

## Get started

- Clone or download this repository to your machine.
  ![clone-repository](.labs/lab1/resources/img/clone-repository.png)
- Ensure you met all the [Requirements](#requirements) before starting the labs.
- Open the lab documents in your browser for an improved reading experience.

## Agenda

| Topic | Content | Labs | Time |
|-------|---------|------|------|
| **Advanced Power BI Development** | • PBIP fundamentals<br>• Automation with AI | [Lab 1 - Power BI Project (PBIP) fundamentals](.labs/lab1/lab1.md)<br>[Lab 2 - PBIP & AI](.labs/lab2/lab2.md)  | 9:00 - 11:30 |
| **Power BI & DevOps** | • Introduction to Git?<br>• Using Git locally with vscode<br>• GitHub Collaboration Workflows<br>• CI/CD Pipelines | [Lab 3 - Introduction to Git with PBIP](.labs/lab3/lab3.md)<br>[Lab 4 - PBIP & CI/CD with GitHub Actions](.labs/lab4/lab4.md) | 13:30 - 17:00 |

## Requirements

- Licensing
  - Access to a Fabric / Power BI tenant
    - You can use your existing organizational tenant but you must have admin permissions.
    - Power BI Pro license
    - Access to a Fabric Capacity or [Trial](https://learn.microsoft.com/en-us/fabric/fundamentals/fabric-trial)
    - We can provide a demo account for you if you dont have access to above.
  - An existing service principal (with `client_id` and `client_secret`) in your Azure tenant or the [permission to create new Entra Application](https://learn.microsoft.com/entra/identity/role-based-access-control/delegate-app-roles) - to be used for CI/CD setup.
  - [Github account](https://github.com/signup)
  - [Sign-up to GitHub Copilot Trial](https://github.com/github-copilot/pro)
- Software
  - [Power BI Desktop](https://pbi.onl/download)
  - [Visual Studio Code](https://code.visualstudio.com/download)
    - Install the following extensions in Visual Studio Code:
      - [TMDL extension](https://marketplace.visualstudio.com/items?itemName=analysis-services.TMDL)
      - [Microsoft Fabric extension](https://marketplace.visualstudio.com/items?itemName=fabric.vscode-fabric)
      - [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot)
      - [GitHub Copilot chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat)
      - [Microsoft PowerShell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell)
      - _Optional_ [GitHub Actions](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-github-actions)
      - _Optional_ [GitHub Pull Requests](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github)
      - _Optional_ [Gitlens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
   - [Git for Windows](https://gitforwindows.org/)

## FAQ

**Q: I don't have access to Power BI license and / or Fabric Capacity.**

**A:** Ask the workshop instructors for a demo account.

---

## Repository Structure

| File/Folder       | Purpose                                                      |
|-------------------|--------------------------------------------------------------|
| `.bpa/`           | Best practice analyzer configuration                         |
| `.devcontainer/`  | Dev Container setup for consistent development environments  |
| `.github/`        | GitHub Actions workflows and GitHub-specific files           |
| `.vscode/`        | VS Code settings and recommended extensions                  |
| `scripts/`        | Helper scripts for automation and deployment                 |
| `src/`            | Main Power BI project files and resources                    |
| `.gitignore`      | Specifies files/folders to be ignored by Git                 |

This repository is organized to help you manage your Power BI project efficiently, especially if you are new to Git, GitHub, and automation with GitHub Actions. Here is a quick guide to the main folders and files you will find:

### `.bpa/`

This folder contains configuration files for static analysis tools such as the [Tabular Editor Best Practice Analyzer](https://docs.tabulareditor.com/te2/Best-Practice-Analyzer-Improvements.html) and [Power BI Inspector (v2)](https://github.com/NatVanG/PBI-InspectorV2). These community tools enable automated testing of Power BI semantic models, reports, and other Microsoft Fabric artifacts against a set of shared best practice rules. By maintaining rule definitions and settings here, you can ensure consistent quality checks and enforce standards across your project using these tools in local development or CI/CD pipelines.

### `.devcontainer/`

Contains configuration files for [Dev Containers](https://containers.dev/), which allow you to develop inside a consistent, pre-configured environment. This is useful for onboarding and ensuring everyone uses the same tools and dependencies.

The provided devcontainer definition at `.devcontainer/devcontainer.json` is preconfigured with recommended vscode extensions as well as the [`fabric-cicd`](https://microsoft.github.io/fabric-cicd/) library and necessary runtimes.

A devcontainer environment can be launched locally with Docker installed, or in the cloud via [GitHub Codespaces](https://github.com/codespaces) - a repository opened in a GitHub Codespace can be edited either from vscode on your desktop or in the browser at <https://github.dev>.

### `.github/`

Holds GitHub-specific files, including workflows for [GitHub Actions](https://docs.github.com/actions). These workflows automate tasks like testing, building, or deploying your project whenever you push changes to the repository.

**Workflow files in `.github/workflows/`:**

| File                | Purpose                                                                 |
|---------------------|-------------------------------------------------------------------------|
| `workflows/deploy.yml`        | Deploys the Power BI/Fabric artifacts to a target workspace and environment. **Triggered on pushes to `main` or manually via workflow dispatch**. Uses a Python deployment script and the `fabric-cicd` library. |
| `workflows/bpa.yml`           | Runs static analysis (Best Practice Analyzer) on semantic models and reports using community tools. **Triggered on pull requests to `main` or manually**. Helps enforce best practices before merging changes. |
| `dependabot.yml`    | Configures [Dependabot](https://docs.github.com/code-security/dependabot) to automate dependency updates, such as for devcontainer definitions. Not a workflow, but part of GitHub automation. |

### `.vscode/`

Contains settings and recommended extensions for [Visual Studio Code](https://code.visualstudio.com/). This helps standardize the development environment for all contributors.

### `scripts/`

This folder includes helper scripts (such as Python or shell scripts) that automate common tasks, like deployment or data processing. You can run these scripts to simplify repetitive work.

This repository contains `scripts/deploy.py`, a Python deployment script that uses [`fabric-cicd`](https://microsoft.github.io/fabric-cicd/).

### `src/`

The main source folder for your Power BI/Fabric project. It contains all the project files, including reports, models, resources, and configuration files.

> [!NOTE]
> Most of your work will happen here.

| File/Folder              | Purpose                                                      |
|--------------------------|--------------------------------------------------------------|
| `Sales.Report/`          | Contains the Power BI report definition, visuals, and resources |
| `Sales.SemanticModel/`   | Contains the semantic model definition, tables, relationships, and metadata |
| `Sales.pbip`             | The main Power BI Project file referencing all artifacts      |
| `parameter.yml`          | Deployment parameters, used by `fabric-cicd` ([docs](https://microsoft.github.io/fabric-cicd/0.1.28/how_to/parameterization/)). The file is never explicitly referenced as it will be discovered in this location automatically. |

### `.gitignore`

This file tells Git which files or folders to ignore (not track). For example, temporary files, build outputs, or sensitive information should be listed here to avoid accidentally sharing them.

`.gitignore` files can be nested. For example, there is `src/.gitignore` with additional ignore rules for PBIP.

---

If you are new to Git or GitHub, don't worry! Each of these folders and files helps organize your project and automate tasks, making collaboration and deployment easier. For more details, check the documentation or ask your team for guidance.
