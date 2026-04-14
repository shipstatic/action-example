# shipstatic/action-example

[![Deploy](https://github.com/shipstatic/action-example/actions/workflows/deploy.yml/badge.svg)](https://github.com/shipstatic/action-example/actions/workflows/deploy.yml)

Example workflows for the [ShipStatic GitHub Action](https://github.com/marketplace/actions/shipstatic) — a React + Vite app deployed to [github-action.shipstatic.com](https://github-action.shipstatic.com).

## [`preview.yml`](.github/workflows/preview.yml) — Free, No Account Needed

Pull requests get a preview deploy with the URL posted as a PR comment. No API key, no sign-up — expires in 3 days.

```yaml
name: Preview
on: pull_request

permissions:
  contents: read
  deployments: write
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

## [`deploy.yml`](.github/workflows/deploy.yml) — Free API Key

Push to `main` deploys permanently with a custom domain. Get a free API key at [my.shipstatic.com/api-key](https://my.shipstatic.com/api-key) and add it as a `SHIP_API_KEY` secret.

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
