# Resourcely Github Actions Scaffolding

This repository demonstrates how to integrate Resourcely into
repository that used Github Actions as the Terraform runner.

It contains a [workflow](.github/workflows/terraform.yml) that runs
`terraform plan` and then uses the [Resourcely Github
Action](https://github.com/Resourcely-Inc/resourcely-action) to
evaluate guardrails on that plan.

## Assumptions

This repository uses Github Actions to run `terraform plan` and
`terraform apply`.

If you use a different runner, see the scaffolding repository for that
runner:

- Terraform Cloud - [scaffolding-github-terraform-cloud](https://github.com/Resourcely-Inc/scaffolding-github-terraform-cloud)

## Usage

This repository is a template. Some setup is required after cloning to use it.

### 1. Configure Terraform Backend

Running Terraform in Github Actions requires storing the Terraform
state in a durable backend.  Terraform supports a variety of backends
described
[here](https://developer.hashicorp.com/terraform/language/settings/backends/configuration).

Edit [terraform.tf](terraform.tf) to add and configured your chosen
backend.

### 2. Add Resourcely API Token to Github Secrets.

The Resourcely Github Action requires an API token to authenticate to
the Resourcely API.

- Generate a new API token in the [Resourcely portal](https://portal.resourcely.io/settings/generate-api-token)
- Create a new [Github repository secret](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions#creating-secrets-for-a-repository) named `RESOURCELY_API_TOKEN` containing this token.

### 3. Update .resourcely.yaml

`.resourcely.yaml` tells Resourcely where to find the Terraform
configs within this repo.  If you move the config out of the
repository root or add new configs in subdirectories, update the file
to reflect these changes.
