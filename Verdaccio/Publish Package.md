# Setup a package on a private Unity Package Manager with Github Actions

The package.json file:

```json
{
  "name": "com.born.borncore",
  "version": "0.10.3",
  "displayName": "BornCore",
  "description": "Core library for BornCore framework.",
  "unity": "2022.3",
  "dependencies": {
    "com.cysharp.unitask": "2.3.3",
    "jp.hadashikick.vcontainer" : "1.13.2"
  }
}
```

Github Workflow file:

`<Project Folder>/.github/workflows/publish-package.yml`

```yaml
name: Publish Package to Born Unity Package Manager

on:
  push:
    branches:
      - master
      - main

defaults:
  run:
    working-directory: Assets/Package
    
jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          lfs: 'true'
      - name: Use Node.js 18.x
        uses: actions/setup-node@v3
        with:
          node-version: 18.x
      - run: npm set registry https://upm.born.net/
      - run: npm config set //upm.born.net/:_authToken "${{ secrets.NPM_AUTH_TOKEN }}"
      - run: npm publish
```

### NPM_AUTH_TOKEN

Add an Actions Secret to your repository (Settings > Secrets and variables > Actions) called `NPM_AUTH_TOKEN`. The contents should be the auth token of a repository admin from https://upm.born.net

## Interim/Patch Releases

On a branch of the package repo, you can publish interim package versions with two steps:

1. Update the `package.json` file to an interim version number:
```json
  "version": "0.10.3-preview.1",
```
2. Temporarily update the `publish-package.yml` workflow to publish the package on push to your branch

```yaml
on:
  push:
    branches:
      - master
      - main
      - fix-something 
```

When you push to your branch, the package will be published to https://upm.born.net
