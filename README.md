# Resourcely Terraform Cloud Scaffolding

This repository demonstrates how to integrate Resourcely into
repository that uses Terraform Cloud as the Terraform runner.

It contains a [workflow](.github/workflows/terraform.yml) that waits
for terraform plan and then uses the [Resourcely Github
Action](https://github.com/Resourcely-Inc/resourcely-action) to
evaluate guardrails on that plan.

## Assumptions

This repository assumes that Terraform Cloud has already been configured to run terraform plan on each pull request.

If you use a different runner, see the scaffolding repository for that
runner:

- Terraform Github Actions - [scaffolding-github-actions](https://github.com/Resourcely-Inc/scaffolding-github-actions)

## Usage

This repository is a template. Some setup is required after cloning to use it.

### 1. Configure Terraform Cloud VCS Integration

To set up Terraform Cloud VCS integration, please refer to the instructions 
[here](https://docs.resourcely.com/getting-started/prerequisites/terraform-integration/terraform-cloud).

Edit [terraform.tf](terraform.tf) to add and configured your chosen
backend.

### 2. Tokens to Github Secrets.

The Resourcely Github Action requires the following tokens to authenticate to
the Resourcely API, Terraform Cloud, and Github.

#### RESOURCELY_API_TOKEN
- Generate a new API token in the [Resourcely portal](https://portal.resourcely.io/settings/generate-api-token)
- Create a new [Github repository secret](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions#creating-secrets-for-a-repository) named `RESOURCELY_API_TOKEN` containing this token.

#### GH_ACCESS_TOKEN
- Resourcely requires a Classic Personal Access Token with the repo scope.
- Go to Github
- In the upper-right corner of any page, click your profile photo, then click Settings.
- In the left sidebar, click  Developer settings.
- In the left sidebar, under  Personal access tokens, click Tokens (classic).
- Select Generate new token, then click Generate new token (classic).
- In the "Note" field, give your token a descriptive name.
- To give your token an expiration, select Expiration, then choose a default option or click Custom to enter a date.
- Select the repo scope
- Click Generate token.

#### TF_API_TOKEN
Your Terraform Cloud team token with workspace permission set to Manage all workspaces.
Please refer to this [section](https://docs.resourcely.com/getting-started/prerequisites/terraform-integration/terraform-cloud#generating-team-api-token) to generate the token. 

### 3. Terraform Cloud VCS Integration Considerations
If you are using Terraform Cloud to provision your infrastructure resources and have performed the following actions:
- Linked your GitHub repository with Terraform Cloud using VCS
- Set the TF_API_TOKEN variable within GitHub

Then the applied Resourcely action will run the wait-for-terraform-plan job which performs the following before verifying the configured guardrails:

- Searches for the Terraform Cloud job run in the Resourcely generated pull-request
- Continuously checks the status of the Terraform Cloud Job until completion
- Downloads the Terraform plans once the Terraform Cloud Job has been completed successfully

### 4. Update .resourcely.yaml

`.resourcely.yaml` tells Resourcely where to find the Terraform
configs within this repo.  If you move the config out of the
repository root or add new configs in subdirectories, update the file
to reflect these changes.
