# ShipStatic Action Example

[![Deploy](https://github.com/shipstatic/action-example/actions/workflows/deploy.yml/badge.svg)](https://github.com/shipstatic/action-example/actions/workflows/deploy.yml)

A React + Vite app deployed to [github-action.shipstatic.com](https://github-action.shipstatic.com) with the [ShipStatic GitHub Action](https://github.com/marketplace/actions/shipstatic).

## Workflow

Pull requests get a free preview deploy — no API key, no account. Push to `main` deploys permanently with a custom domain.

```yaml
name: Deploy
on:
  push:
    branches: [main]
  pull_request:

jobs:
  # Free tier — no account needed, expires in 3 days
  preview:
    if: github.event_name == 'pull_request'
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    steps:
      - uses: actions/checkout@v4
      - run: npm ci && npm run build
      - uses: shipstatic/action@v1
        with:
          path: ./dist
          github-token: ${{ secrets.GITHUB_TOKEN }}

  # With API key — permanent deploy + custom domain
  production:
    if: github.event_name == 'push'
    runs-on: ubuntu-latest
    permissions:
      contents: read
      deployments: write
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
