## Usage:

```yml
name: Preview Environment

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
      - id: snaplet
        uses: snaplet/vercel-snaplet@v1
```