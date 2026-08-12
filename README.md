# shipstatic/action-example

[![Deploy](https://github.com/shipstatic/action-example/actions/workflows/deploy-domain.yml/badge.svg)](https://github.com/shipstatic/action-example/actions/workflows/deploy-domain.yml)

Example workflows for the [ShipStatic GitHub Action](https://github.com/marketplace/actions/shipstatic) — a React + Vite app deployed to [action-example.shipstatic.com](https://action-example.shipstatic.com).

Every one of them writes a summary to its workflow run page — the deployment, its URL, the domain when one is linked, and for deploys made without an API key the claim link and expiry. Nothing to configure.

## [`deploy-no-account.yml`](.github/workflows/deploy-no-account.yml) — Free, No Account Needed

Push to `main` deploys instantly. No API key, no sign-up, no configuration — expires in 3 days.

```yaml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - run: npm ci && npm run build
      - uses: shipstatic/action@v2
        with:
          path: ./dist
```

## [`deploy-api-key.yml`](.github/workflows/deploy-api-key.yml) — Free API Key

Push to `main` deploys permanently. Get a free API key at [my.shipstatic.com/api-key](https://my.shipstatic.com/api-key) and add it as a `SHIP_TOKEN` secret.

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
      - uses: actions/checkout@v7
      - run: npm ci && npm run build
      - uses: shipstatic/action@v2
        with:
          token: ${{ secrets.SHIP_TOKEN }}
          path: ./dist
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

## [`deploy-domain.yml`](.github/workflows/deploy-domain.yml) — Free API Key + Custom Domain

Push to `main` deploys permanently and links a custom domain. Set a `DOMAIN` variable with your domain name (Settings > Variables > Actions).

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
      - uses: actions/checkout@v7
      - run: npm ci && npm run build
      - uses: shipstatic/action@v2
        with:
          token: ${{ secrets.SHIP_TOKEN }}
          path: ./dist
          domain: ${{ vars.DOMAIN }}
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

## [`preview-pr.yml`](.github/workflows/preview-pr.yml) — PR Preview

Every pull request gets a preview deploy with the URL posted as a comment — one comment per PR, updated in place on every push rather than a new one each time. Works without an API key — expires in 3 days, and the comment carries the claim link that keeps it.

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
      - uses: actions/checkout@v7
      - run: npm ci && npm run build
      - uses: shipstatic/action@v2
        with:
          path: ./dist
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

## [`deploy-password.yml`](.github/workflows/deploy-password.yml) — Password-Protected Deploy

Push to `main` deploys with an unlock prompt — visitors enter the password before viewing. Works on any tier (no API key required). Set a `SHIP_PASSWORD` secret (6–128 characters) and share it out-of-band with the people who should see the site.

```yaml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - run: npm ci && npm run build
      - uses: shipstatic/action@v2
        with:
          path: ./dist
          password: ${{ secrets.SHIP_PASSWORD }}
```

---

See the [action documentation](https://github.com/shipstatic/action) for all inputs, outputs, and options.
