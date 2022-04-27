# Vue Template

## Getting Started

- From GitHub click "Use this template"

### Package install

To install:

`npm install`

### Running local dev server

To run a development server using (`./envs/.env.development` vars):

`npm run dev`

To run a development server using (`./envs/.env.production` vars):

`npm run dev:production`

### Building for Deployment

To make a production build using (`./envs/.env.development` vars):

`npm run build:development`

To make a production build using (`./envs/.env.production` vars):

`npm run build:production`

To serve a production build locally for testing:

`npm run serve`

## Testing

### Open Cypress UI

`npm run cy:open`

### Run Cypress Tests from Command Line

`npm run cy:run`

## Linting and Formatting

ESLint and Prettier are employed for linting and formatting. Implemented following this guide:
https://vueschool.io/articles/vuejs-tutorials/eslint-and-prettier-with-vite-and-vue-js-3/

It's recommended you install the following VSCode Extensions:

- [ESLint](https://open-vsx.org/extension/dbaeumer/vscode-eslint) VSCode extension.
- [Volar](https://marketplace.visualstudio.com/items?itemName=johnsoncodehk.volar)

### Manual Lint w/ ESLint

`npm run lint`

### Manual Format w/ Prettier

`npm run format`

## Deployment - Azure Blobs

https://vue-template.iatistandard.org/

This method should be used for our live sites that require `dev` and `prod` sites

### Storage Account Setup

In Azure Portal

- Create a resource group `rg-<yourapp>-<env>`
- Create a Storage account `sa<yourapp><env>`
  - Accept defaults
- In the Storage Account go to > Data Management > Static Website
  - Select "Enabled"
  - Add `index.html` to Index/Error document name respectively
  - Click "Save"
- Go to Networking > Custom Domain Tab
  - You can only create domain names with a single subdomain depth, e.g. `dev-hello.iatistandard.org`. `dev.hello.iatistandard.org` will not work on our current Cloudflare plan due to the way the certificate is configured. [reference](https://community.cloudflare.com/t/subdomain-too-deep/81872)
  - Follow instructions to add CNAME records in Cloudflare DNS
    - ENSURE YOU USE THE URL with `z33` in it: e.g. `saangulartemplatedev.z33.web.core.windows.net`
    - When you first add the CNAMEs ensure they have a grey cloud "DNS Only" (are NOT proxied)
  - Add the custom domain into the Azure Portal field and Click Save
  - Back in Cloudflare set the custom domain (non asverify one) to be Proxied (orange cloud)
- Get the Storage account Access key from Azure Portal > Access Keys > Key
- Save the key into a GitHub secret in your repo e.g. DEV_STORAGE_KEY
- See `develop_CD.yml` for the GitHub Actions workflow. The workflow assumes your app is built into `/dist`, which vite does by default

### Cloudflare Set Up

#### HTTPS

- Navigate to Rules -> Page Rules in Cloudflare
- Create a new page rule for the url (e.g. iativalidator.iatistandard.org/\* )
- Select "Always Use HTTPS"

#### Rewrite to index.html for SPA

If a user/bot/analytics navigates directly to a path (e.g. iativalidator.iatistandard.org/organisations ), the default setup in Azure Blobs will return a 404 before redirecting to the correct resources. This is a problem for bots/analytics. It works okay in the browser but still shows an error in the console. Vue JS expects to always be directed to index.html and then handle routing internally.

Fix:

- Navigate to Rules -> Transform Rules in Cloudflare
- Create a new Transform rule of type "URL Rewrite"
  - Field: Hostname, Operator: equals, Value: e.g. "iativalidator.iatstandard.org"
  - AND
  - Field URI Path, Operator: does not contain, Value: .
  - Then...
  - Rewrite to Static /index.html
- Save

# Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

## Recommended IDE Setup

- [VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=johnsoncodehk.volar)
