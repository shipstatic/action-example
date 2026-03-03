# Shipstatic Action Example

[![Deploy](https://github.com/shipstatic/action-example/actions/workflows/deploy.yml/badge.svg)](https://github.com/shipstatic/action-example/actions/workflows/deploy.yml)

A React + Vite app deployed to [github-action.shipstatic.com](https://github-action.shipstatic.com) with the [Shipstatic GitHub Action](https://github.com/marketplace/actions/shipstatic).

## Workflow

```yaml
name: Deploy
on:
  push:
    branches: [main]
  pull_request:

permissions:
  contents: read
  deployments: write
  pull-requests: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Build
        run: npm ci && npm run build

      - name: Deploy
        id: deploy
        uses: shipstatic/action@v1
        with:
          api-key: ${{ secrets.SHIP_API_KEY }}
          path: ./dist
          domain: ${{ github.event_name == 'push' && 'github-action.shipstatic.com' || '' }}
          github-token: ${{ secrets.GITHUB_TOKEN }}

      - name: Summary
        run: echo "Deployed to ${{ steps.deploy.outputs.url }}" >> "$GITHUB_STEP_SUMMARY"
```

Push to `main` deploys and links to the domain. Pull requests get a preview deploy with the URL posted as a comment.
