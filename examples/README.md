# Examples

These folders are the examples of the Weik.io docs, the University videos and the live demo. `hello` is the integration that `weikio init hello` writes, and `finance` is a project with one example of each kind: a Camel integration, a transfer, an API and a channel.

The `finance` folder is laid out for GitOps. The documents every environment shares are in their own folders, and `devtest/` and `production/` contain what differs per environment. `project.yaml` and `variables/` belong to a platform administrator, because they change every environment at once.

## Apply them to your own installation

1. Install the CLI and sign in to your Weik.io installation:

   ```sh
   curl -fsSL https://weik.io/install.sh | sh
   weikio login https://hub.example.com
   ```

   The first command needs `curl` and `ca-certificates`, which minimal images such as `debian:12` don't have, so install them there first with `apt-get update && apt-get install -y curl ca-certificates`.

2. As a platform administrator, create the project and its default values:

   ```sh
   weikio apply -f ./finance/project.yaml -f ./finance/variables
   ```

3. Set the secrets the examples read. Each command asks for the value with hidden input:

   ```sh
   weikio secret set finance/erp-api-key --env devtest
   weikio secret set finance/supplier-ftp-password --env devtest
   weikio secret set finance/invoice-archive-key --env devtest
   ```

4. Apply the shared folders and the devtest folder, which publishes a version of each integration and starts `erp-invoice-export` in devtest:

   ```sh
   weikio apply --env devtest --prune finance \
     -f ./finance/erp-invoice-export -f ./finance/transfers -f ./finance/apis \
     -f ./finance/invoice-created -f ./finance/devtest
   ```

To run the same steps from GitHub Actions, copy `.github/workflows/` to the root of your own repository, set `WEIKIO_SERVER` in the workflows to the installation's address, and add a `WEIKIO_API_KEY` secret to the GitHub environments `devtest`, `production-plan` and `production`. Each key needs the `deployer` role on `finance` in its own environment only, and `production` gets required reviewers. Set `finance/erp-api-key` with `--env production` by hand before the first production run. Promotion is then a pull request that changes the version in `finance/production/deployments/erp-invoice-export.yaml`.

The guide to keeping integrations in Git is https://weik.io/docs/deliver/gitops.
