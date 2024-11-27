# Reflex Deploy Action
This GitHub Action simplifies the deployment of Reflex applications to Reflex Cloud. It handles setting up the environment, installing the Reflex CLI, and deploying your app with minimal configuration.

> [!WARNING]
> This action requires reflex version 0.6.6 or higher

**Features:**
- Deploy Reflex apps directly from your GitHub repository to Reflex Cloud.
- Supports subdirectory-based app structures.
- Securely uses authentication tokens via GitHub Secrets.

Usage
## Add the Action to Your Workflow
Create a .github/workflows/deploy.yml file in your repository and add the following:

```yaml
Copy code
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
        uses: your-username/reflex-deploy-action@v1
        with:
          auth_token: ${{ secrets.REFLEX_AUTH_TOKEN }}
          app_directory: "my-app-folder" # Optional, defaults to root
```
## Set Up Your Secrets
Store your Reflex authentication token securely in your repository's secrets:

1. Go to your GitHub repository.
2. Navigate to Settings > Secrets and variables > Actions > New repository secret.
3. Add a new secret with the name `REFLEX_AUTH_TOKEN` and paste your Reflex Cloud authentication token.


### Inputs
| Name | Description | Required | Default |
|:- |:- |:- |:- |
|auth_token   | Reflex authentication token stored in GitHub Secrets. | ✅ |	N/A |
|project_id   | The ID of the project you want to deploy to. |	✅ |	N/A |
|app_directory|	The directory containing your Reflex app.|	❌	| . (root)|

