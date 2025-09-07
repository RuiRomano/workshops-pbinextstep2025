TBD

This repository showcases a Power BI Project (PBIP) folder structure in the [src folder](./src/), featuring multiple reports and semantic models.

```mermaid
graph TD
  Report01.Report --> Model01.SemanticModel
  Report02.Report --> Model01.SemanticModel
  Report03.Report --> Model01.SemanticModel
  Report04.Report --> Model02.SemanticModel
  Report05.Report --> Model03.SemanticModel

```

- Create a [Service Principal](https://learn.microsoft.com/en-us/rest/api/fabric/articles/get-started/create-entra-app) and configure required [Github secrets and variables](#secrets-and-variables) in your repo if you wish to test deployment from GitHub.
- Create the target Fabric Workspace and add the service principal as Admin.
- Run the [deploy](/.github/workflows/deploy.yml) Github action to deploy content to the workspace.

## Local deployment with script and fabric-cicd

- Make sure you have the [Fabric CICD](https://microsoft.github.io/fabric-cicd/latest/) installed. If not, run:
    ```bash
    $ pip install fabric-cicd 
    ```
- Create the target Fabric Workspace
- Run the [deploy.py](./deploy.py)

## Run Best Practice Analysis

- Manually [bpa](/.github/workflows/bpa.yml) Github action or after a pull-request against main branch
- You can also run locally by running the script [bpa.ps1](/.bpa/bpa.ps1)
  
- ### Secrets and variables

Before running the Github Action, ensure you configure the following [GitHub action secrets and variables](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions):

| Name            | Type   | Value                |
|-----------------|--------|----------------------|
| `FABRIC_CLIENT_ID`       | Secret | [Service Principal](https://learn.microsoft.com/en-us/entra/identity-platform/howto-create-service-principal-portal) client ID from your tenant   |
| `FABRIC_CLIENT_SECRET`   | Secret | Service Principal secret |
| `FABRIC_TENANT_ID` | Secret | Your tenant ID |