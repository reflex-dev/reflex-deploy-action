# Reflex Deploy Action

This GitHub Action simplifies the deployment of Reflex applications to Reflex Cloud by invoking `reflex deploy` with the provided credentials.

> [!WARNING]
> This action requires reflex>=0.6.6.post3. If you are using an older version, please update your Reflex CLI.

> [!NOTE]
> This action expects the repository to already be checked out and the Reflex CLI to be installed in the runner environment. Use `actions/checkout` and `actions/setup-python` (plus `pip install -r requirements.txt`) in earlier workflow steps.

**Features:**

- Deploy Reflex apps directly from your GitHub repository to Reflex Cloud.
- Supports subdirectory-based app structures.
- Securely uses authentication tokens via GitHub Secrets.

## Usage

### Add the Action to Your Workflow

Create a .github/workflows/deploy.yml file in your repository and add the following:

```yaml
name: Deploy Reflex App

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      # Pinning third-party actions to a full commit SHA protects against
      # supply chain attacks where a tag (e.g. `v6`) is re-pointed at
      # malicious code. Check the action's repo for the latest release SHA.
      - uses: actions/checkout@de0fac2e4500dabe0009e67214ff5f5447ce83dd # v6.0.2
      - uses: actions/setup-python@a309ff8b426b58ec0e2a45f0f869d46889d02405 # v6.2.0
        with:
          python-version: "3.12"
      - name: Install Reflex
        run: pip install -r my-app-folder/requirements.txt
      - name: Deploy to Reflex Cloud
        uses: reflex-dev/reflex-deploy-action@v3
        with:
          auth_token: ${{ secrets.REFLEX_AUTH_TOKEN }}
          project_id: ${{ secrets.REFLEX_PROJECT_ID }}
          app_directory: "my-app-folder" # Optional, defaults to root
          extra_args: "--env THIRD_PARTY_APIKEY=${{ secrets.THIRD_PARTY_APIKEY }}" # Optional
```

### Set Up Your Secrets

Store your Reflex secrets securely in your repository:

1. Go to your GitHub repository.
2. Navigate to Settings > Secrets and variables > Actions > New repository secret.
3. Create new secrets for `REFLEX_AUTH_TOKEN` and `REFLEX_PROJECT_ID`. (See [the deploy docs](https://reflex.dev/docs/hosting/deploy/) for more information on how to get these values.)

### Inputs

| Name          | Description                                                  | Required | Default  |
| :------------ | :----------------------------------------------------------- | :------- | :------- |
| auth_token    | Reflex authentication token stored in GitHub Secrets.        | ✅       | N/A      |
| project_id    | The ID of the project you want to deploy to.                 | ✅       | N/A      |
| app_directory | The directory containing your Reflex app.                    | ❌       | . (root) |
| extra_args    | Additional arguments to pass to the `reflex deploy` command. | ❌       | N/A      |

## Migrating from v2

v3 no longer checks out your repo, installs Python, or installs Reflex — add those steps yourself (see [Usage](#add-the-action-to-your-workflow)).

Removed inputs:

- `python_version` — use `actions/setup-python`.
- `dry_run` — skip the step with an `if:` condition instead.
- `skip_checkout` — just omit `actions/checkout`.
