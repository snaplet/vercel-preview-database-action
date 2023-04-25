# Vercel Preview Database Github Action

<p align="center">
  <img width="360" src="logo.png" alt="Snappy looking through a magnifier with the Vercel logo in its center">
</p>


> Deploy your web with Vercel and preview databases with  Snaplet filled with production-accurate data in no time.

With this action, you can finally enjoy fully isolated and stateful preview environments. Get a new preview database filled with production-accurate data anytime you need it and plug it into your Vercel preview process.

## Usage

Create a GitHub Action Workflow file in your repository following one of these examples.

```yaml
# .github/workflows/preview.yml

name: Preview

env:
  SNAPLET_ACCESS_TOKEN: ${{ secrets.SNAPLET_ACCESS_TOKEN }}
  SNAPLET_PROJECT_ID: <YOUR_SNAPLET_PROJECT_ID>
  VERCEL_ACCESS_TOKEN: ${{ secrets.VERCEL_ACCESS_TOKEN }}
  VERCEL_PROJECT_ID: <YOUR_VERCEL_PROJECT_ID>

on:
  pull_request:
    types: [opened, synchronize, closed]
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: snaplet/vercel-preview-database-action@v1
```

## Documentation

### Prerequisites
[Connect your GitHub repository with Vercel](https://vercel.com/docs/concepts/git/vercel-for-github)
[Fetch your Snaplet credentials](https://docs.snaplet.dev/guides/netlify-preview-plugin/#step-3-add-environment-variables)
[Fetch your Vercel Access token](https://vercel.com/account/tokens)

### Environment variables
- SNAPLET_ACCESS_TOKEN **[required]**
- SNAPLET_PROJECT_ID **[required]**
- VERCEL_ACCESS_TOKEN **[required]**
- VERCEL_PROJECT_ID **[required]**

### Inputs

```yaml
  # Mixed action inputs
  delete:
    description: Delete the preview on vercel and the instant database related to it
    required: false
    type: boolean
    default: ${{ github.event.action == 'closed' }}
  # Vercel action inputs
  vercel-await-for-deployment:
    description: Await for the deployment to be ready and output the deployment URL
    required: false
    type: boolean
    default: false
  vercel-env:
    description: The environment variable name to set the preview database deployment URL
    required: false
    type: string
    default: DATABASE_URL
  vercel-ignored-build-command:
    description: Command set for the Ignored Build Step in your project settings, the default script is canceling every preview deployments coming from the Vercel GitHub App.
    required: false
    type: string
    default: curl -sS "https://raw.githubusercontent.com/snaplet/vercel-action/v3/scripts/ignore-build.mjs" | node --input-type=module
  # Snaplet action inputs
  database-create-command:
    description: Command used to generate the instant database
    required: false
    type: string
    default: snaplet database create --git --latest
  database-delete-command:
    description: Command used to delete the instant database
    required: false
    type: string
    default: snaplet database delete --git
  database-url-command:
    description: Command used to get the instant database url
    required: false
    type: string
    default: snaplet database url --git
  database-reset:
    description: Reset the database state on each commit
    required: false
    type: boolean
    default: false
```

### Outputs

```yaml
database-url:
  description: Instant database url
deployment-url:
  description: Vercel preview deployment url
```