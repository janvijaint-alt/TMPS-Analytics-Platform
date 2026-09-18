# TMPS Analytics Platform

Production-oriented Azure analytics platform scaffold using the Medallion Architecture:

- **Bronze:** immutable raw landing in ADLS Gen2.
- **Silver:** validated, deduplicated Delta tables in Databricks.
- **Gold:** governed dimensional and aggregate tables for Power BI.

## Repository layout

```text
infrastructure/   Terraform for Azure resources, RBAC, Key Vault, storage, Databricks
orchestration/    Azure Data Factory linked services, datasets, and pipelines
streaming/        Kafka contracts and producer/consumer starter code
databricks/       PySpark transformations and Delta/streaming jobs
ai_foundry/       Prompt templates and evaluation starter scripts
bi_reporting/     Power BI semantic model and DAX starter assets
tests/             Unit tests for transformation logic
.github/workflows CI/CD validation and environment deployment
```

## Security principles

- Authentication uses Azure Managed Identity and Microsoft Entra ID.
- Secrets are resolved from Azure Key Vault; no credentials are committed.
- Unity Catalog is the governance boundary for Databricks data.
- Terraform state must be stored in a secured, private Azure Storage backend.
- RBAC is least-privilege and should be reviewed before production rollout.

## Deployment

1. Bootstrap a remote Terraform state storage account and configure `infrastructure/backend.tf`.
2. Configure GitHub OIDC federation for the deployment service principal.
3. Add environment variables/secrets in GitHub Environments (`dev`, `test`, `prod`).
4. Run `terraform init`, `terraform plan`, and `terraform apply` from the selected environment.
5. Import ADF assets into the target factory and configure managed private endpoints as required.

See `docs/architecture.md` and `docs/deployment.md` for the operating model.
