# GitHub Action: Publish to NPM and GitHub

Publishes an NPM package described via `package.json` to GitHub and NPM
simultaniously with different package names.

## Usage

You must have a NPM token with the ability to publish packages. It is
recommended that you store this in your repository's secrets.

Example workflow:

```yml
name: Publish package

on:
  release:
    types: [published]

jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
      id-token: write

    steps:
      - name: Check out code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20.x'

      - name: Install dependencies and build
        run:  |
          npm ci
          npm run build

      - name: Publish package to GitHub and NPM
        uses: ava-cassiopeia/publish-to-npm-and-github@1.0.0
        with:
          npm-package-name: 'your-npm-package-name'
          github-package-name: '@your-github-username/your-gh-package-name'
          npm-token: ${{ secrets.NPM_TOKEN }}
          github-token: ${{ secrets.GITHUB_TOKEN }}
```
