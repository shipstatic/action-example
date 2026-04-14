# shipstatic/action-example

[![Deploy](https://github.com/shipstatic/action-example/actions/workflows/deploy-domain.yml/badge.svg)](https://github.com/shipstatic/action-example/actions/workflows/deploy-domain.yml)

Example workflows for the [ShipStatic GitHub Action](https://github.com/marketplace/actions/shipstatic) — a React + Vite app deployed to [github-action.shipstatic.com](https://github-action.shipstatic.com).

## [`deploy-no-account.yml`](.github/workflows/deploy-no-account.yml) — Free, No Account Needed

Push to `main` deploys instantly. No API key, no sign-up, no configuration — expires in 3 days.

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
      - uses: shipstatic/action@v1
        with:
          path: ./dist
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

## [`deploy-api-key.yml`](.github/workflows/deploy-api-key.yml) — Free API Key

Push to `main` deploys permanently. Get a free API key at [my.shipstatic.com/api-key](https://my.shipstatic.com/api-key) and add it as a `SHIP_API_KEY` secret.

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
      - uses: shipstatic/action@v1
        with:
          api-key: ${{ secrets.SHIP_API_KEY }}
          path: ./dist
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

## [`deploy-domain.yml`](.github/workflows/deploy-domain.yml) — Free API Key + Custom Domain

Push to `main` deploys permanently and links a custom domain.

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

---

See the [action documentation](https://github.com/shipstatic/action) for all inputs, outputs, and options.
