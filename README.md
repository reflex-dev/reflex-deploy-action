# Reflex Deploy Action
This GitHub Action simplifies the deployment of Reflex applications to Reflex Cloud. It handles setting up the environment, installing the Reflex CLI, and deploying your app with minimal configuration.

> [!WARNING]
> This action requires reflex>=0.6.6.post3. If you are using an older version, please update your Reflex CLI.

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
      - name: Deploy to Reflex Cloud
        uses: reflex-dev/reflex-deploy-action@v1
        with:
          auth_token: ${{ secrets.REFLEX_AUTH_TOKEN }}
          project_id: ${{ secrets.REFLEX_PROJECT_ID }}
          app_directory: "my-app-folder" # Optional, defaults to root
          extra_args: "--env THIRD_PARTY_APIKEY=${{ secrets.THIRD_PARTY_APIKEY }}" # Optional
          python_version: "3.12" # Optional
          dry_run: "false" # Optional
```

### Set Up Your Secrets
Store your Reflex secrets securely in your repository:

1. Go to your GitHub repository.
2. Navigate to Settings > Secrets and variables > Actions > New repository secret.
3. Create new secrets for `REFLEX_AUTH_TOKEN` and `REFLEX_PROJECT_ID`. (See [the deploy docs](https://reflex.dev/docs/hosting/deploy/) for more information on how to get these values.)

### Inputs
|     Name     | Description                                                  | Required | Default |
|:------------ |:------------------------------------------------------------ |:-------- |:------- |
|auth_token    | Reflex authentication token stored in GitHub Secrets.        |    ✅    |	N/A   |
|project_id    | The ID of the project you want to deploy to.                 |    ✅    |   N/A   |
|app_directory | The directory containing your Reflex app.                    |	   ❌    | . (root)|
|extra_args	   | Additional arguments to pass to the `reflex deploy` command. |	   ❌    |   N/A   |
|python_version| The Python version to use for the deployment environment.    |	   ❌    |   3.12  |
|dry_run       | Whether to run the deployment in dry-run mode.               |	   ❌    |  false  |
|skip_checkout | Whether to skip checking out the code, useful for deploying local changes made by an earlier pipeline step.               |	   ❌    |  false  |

