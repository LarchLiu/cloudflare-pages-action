# Cloudflare Pages Action

Deploy your project to Cloudflare Pages with automatic project creation and setup custom domain (with support multiple domain)

## API Token Permissions

  * Account Resources:
    * Resource: Include -> All Accounts (or spesific account)
    * Permissions: Pages -> Edit

  * Zone Resources (if using custom domains):
    * Resource: Include -> All Zones (or spesific zone)
    * Permissions: Zone -> Read, DNS -> Edit

## Usage

### Deploy preview

```yaml
on:
  pull_request:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    name: Deploy
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Build
        run: pnpm build

      - name: Deploy
        uses: kitabisa/cloudflare-pages-action@v2
        with:
          api-token: ${{ secrets.CLOUDFLARE_API_TOKEN }}
          account-id: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
          branch: ${{ github.ref }}
          production-branch: main
          package-manager: pnpm
          build-directory: ./out
          project-name: your-cloudflare-project
          custom-domains: dev-${{ github.event.pull_request.number }}.example.com
          working-directory: ./
```

### Deploy production

```yaml
on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    name: Deploy
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Build
        run: pnpm build

      - name: Deploy
        uses: kitabisa/cloudflare-pages-action@v2
        with:
          api-token: ${{ secrets.CLOUDFLARE_API_TOKEN }}
          account-id: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
          branch: main
          production-branch: main
          package-manager: pnpm
          build-directory: ./out
          project-name: your-cloudflare-project
          custom-domains: example.com,example.co.id,example.id
          working-directory: ./
```

## Inputs

| Name                  | Description                       | Default       | Examples                              |
| --------------------- | --------------------------------- | ------------- | ------------------------------------- |
| `api-token`           | Define Cloudflare api token.      | `undefined`   | `sjcbjxbBJKBJKBbkjbjkbkBkbjkBjkbk`    |
| `account-id`          | Define Cloudflare account id.     | `undefined`   | `023e105f4ecef8ad9ca31a8372d0c353`    |
| `branch`              | Branch for current deployment.    | `undefined`   | `${{ github.ref }}`                   |
| `production-branch`   | Branch for production deployment. | `undefined`   | `main`                                |
| `package-manager`     | Setup package manager.            | `undefined`   | `npm`, `yarn`, `pnpm`, `bun`          |
| `build-directory`     | Define output build directory.    | `undefined`   | `./out`                               |
| `project-name`        | Setup project name.               | `undefined`   | `kitabisa-accounts`                   |
| `custom-domains`      | Setup custom domains.             | `""`          | `accounts.kitabisa.com`               |
| `working-directory`   | Setup working directory.          | `"."`         | `./apps/accounts`                     |

## Outputs

| Name              | Description                                               | Example                       |
| ----------------- | --------------------------------------------------------- | ----------------------------- |
| `deployment-url`  | The output deployment url from custom domains (if set).   | `accounts.kitabisa.com`       |
| `pages-url`       | The output deployment url from cloudflare pages.          | `kitabisa-accounts.pages.dev` |
