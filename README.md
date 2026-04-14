# ShipStatic Action Example

[![Deploy](https://github.com/shipstatic/action-example/actions/workflows/deploy.yml/badge.svg)](https://github.com/shipstatic/action-example/actions/workflows/deploy.yml)

A React + Vite app deployed to [github-action.shipstatic.com](https://github-action.shipstatic.com) with the [ShipStatic GitHub Action](https://github.com/marketplace/actions/shipstatic).

## Preview — Free, No Account Needed

Pull requests get a temporary preview deploy. No API key, no sign-up.

```yaml
name: Preview
on: pull_request

permissions:
  contents: read
  pull-requests: write

jobs:
  preview:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci && npm run build
      - uses: shipstatic/action@v1
        with:
          path: ./dist
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

## Deploy — With API Key

Push to `main` deploys permanently with a custom domain.

```yaml
name: Deploy
on:
  push:
    branches: [main]

permissions:
  contents: read
  deployments: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci && npm run build
      - id: deploy
        uses: shipstatic/action@v1
        with:
          api-key: ${{ secrets.SHIP_API_KEY }}
          path: ./dist
          domain: github-action.shipstatic.com
          github-token: ${{ secrets.GITHUB_TOKEN }}
      - run: echo "Deployed to ${{ steps.deploy.outputs.url }}" >> "$GITHUB_STEP_SUMMARY"
```
